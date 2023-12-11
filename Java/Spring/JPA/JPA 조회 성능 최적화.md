# JPA 조회 성능 최적화
## 엔티티를 직접 사용
### 1. 양방향 관계에서의 조회
```java
@Entity
@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Order {
    @Id 
    @GeneratedValue
    @Column(name = "order_id")
    private int id;
    
    @ManyToOne(fetch = LAZY)
    @JoinColumn(name = "member_id")
    private Member member;
    
    private LocalDateTime orderDate;
    
    // ...
}

@Entity
@Getter
@Setter
public class Member {
    @Id
    @GeneratedValue
    @Column(name = "member_id")
    private Long id;
    
    private String name;
    
    @JsonIgnore
    @Embedded
    private Address address;
    
    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();
}
```
* 위와 같이 Entity 객체들이 존재하고, `Order` 엔티티를 전체 조회하는 부분이 있다고 해본다.

```java
List<Order> allOrder = orderRepository.findAllByString(new OrderSearch());
```

* 해당 조회 로직을 실행하는 순간 **무한루프 에러**가 발생하면서 에러가 발생할 것이다.
  * `Order` 엔티티의 필드에 ManyToOne 관계로 연관된 `Member` 엔티티가 있고, `Member` 엔티티에는 OneToMany로 연관된 `Order` 엔티티가 있어 Json을 만들면서 계속 엔티티 객체를 만들기 때문이다.

#### 해결방안
* 양방향 연관관계가 있다면 두 부분 중 하나에는 `@JsonIgnore`를 설정해줘야 한다.
```java
@Entity
@Getter
@Setter
public class Member {
    @Id
    @GeneratedValue
    @Column(name = "member_id")
    private Long id;
    
    private String name;
    
    @JsonIgnore
    @Embedded
    private Address address;
    
    @JsonIgnore     // 멤버 내 Order를 조회하는 부분에 JsonIgnore
    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();
}
```

### 2. 지연로딩 (`fetch = LAZY`) 문제
* 지연로딩으로 설정되어 있는 연관관계 엔티티는 실제 DB에서 가져오는 객체가 아닌 프록시 객체를 생성하게 된다.
  * 주로 `byteBuddy` 라는 라이브러리로 만든 프록시 객체를 사용한다.
```java
@Entity
@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Order {
	@Id
	@GeneratedValue
	@Column(name = "order_id")
	private int id;

	@ManyToOne(fetch = LAZY)    // 지연로딩으로 설정되어 있으므로 Member에는 프록시 멤버 객체가 들어간다.
	@JoinColumn(name = "member_id")
	private Member member;

	private LocalDateTime orderDate;
}
```

* 이렇게 프록시 객체로 들어가게 된 엔티티로 인해, `Order` 엔티티를 조회할 때 JPA는 `Member` 엔티티 객체를 가져오고 싶어하는데 프록시 객체가 등록되어 있어 **에러**가 발생하게 된다.

#### 해결방안
* Hibernate5 라이브러리를 설치하고, `FORCE_LAZY_LOADING` 설정으로 강제로 엔티티를 로딩되도록 한다.
  * 하지만 **별로 추천하는 방법은 아니다.**
    * 지연로딩 설정한 엔티티들을 모두 강제로 로딩하는 방법이라 성능 이슈가 발생할 수 있다.
* 로직에서 원하는 컬럼값만 조회하도록 하기
```java
List<Order> allOrder = orderRepository.findAllByString(new OrderSearch());
for (Order order : allOrder) {
	// 실제 컬럼값을 가져오면 LAZY가 강제 초기화된다.
	order.getMember().getName();
	order.getDelivery().getAddress();
}
```

### 참고
* 엔티티를 API 응답으로 외부에 노출하는 것은 좋지 않다. 따라서 DTO로 변환해서 사용하는 것이 더 좋은 방법이다.
* 지연로딩을 피하기 위해서 `EAGER`로 설정하는 것은 안 된다. (n + 1 문제) 항상 연관관계는 지연로딩으로 설정하고, 성능 최적화가 필요한 경우에는 패치 조인(fetch join)을 사용한다.

## 엔티티를 DTO로 변환
```java
// 클라이언트와 협의한 API 스펙대로 DTO 생성
@Data
public class SimpleOrderDto {
    private Long orderId;
    private String name;
    private LocalDateTime orderDate;
    private OrderStatus orderStatus;
    private Address address;
    
    // DTO가 Entity를 파라미터로 받는 건 문제가 되지 않음
    public SimpleOrderDto(Order order) {
        orderId = order.getId();
        name = order.getMember().getName();     // LAZY 초기화
        orderDate = order.getOrderDate();
        orderStatus = order.getStatus();
        address = order.getDelivery().getAddress();     // LAZY 초기화
    }
}
```
```java
List<Order> orders = orderRepository.findAllByString(new OrderSearch());
List<SimpleOrderDto> dtos = orders.stream()
        .map(o -> new SimpleOrderDto(o))
        .toList();
```

* 위와 같이 조회하게 될 때, 조회한 `Order` 개수가 n개라면 실제 실행되는 쿼리는 1 + N + N 번 만큼 실행될 것이다.
  * 1 = `findAll`로 Order를 모두 조회하는 쿼리
  * N = `Member` 테이블에서 `Order`와 매칭되는 name 조회하는 쿼리 (`order → member` 지연로딩 조회 N번)
  * N = `Delivery` 테이블에서 `Order`와 매칭되는 address 조회하는 쿼리 (`order → delivery` 지연로딩 조회 N번)
* 필요하다면 패치 조인으로 튜닝할 수 있다.

## 페치 조인 최적화
```java
// 페치 조인을 사용한 조회 메서드
public List<Order> findAllWithMemberDelivery() {
    return entityManager.createQuery(
            "select o from Order o" +
            "join fetch o.member m" +
            "join fetch o.delivery d", Order.class
        ).getResultList();
}

// 위 메서드를 사용하여 API 로직 구현하기
List<Order> orders = orderRepository.findAllWithMemberDelivery();
List<SimpleOrderDto> dtos = orders.stream()
    .map(o -> new SimpleOrderDto(o))
    .toList();
```

* 페치 조인을 사용하면 쿼리 1번만에 모두 조회 가능하다.
* 페치 조인으로 `order → member`, `order → delivery` 는 이미 조회 된 상태이므로 지연로딩은 일어나지 않는다.

## JPA에서 DTO 바로 조회하기
```java
public class OrderRepository {
    // ...
    
    public List<OrderSimpleQueryDto> findOrderDtos() {
        entityManager.createQuery(
                    "select new jpabook.jpashop.repository.OrderSimpleQueryDto(o.id, m.name, o.orderDate, o.status, d.address) from Order o" +
                    "join o.member m" +
                    "join o.delivery d", OrderSimpleQueryDto.class)
                .getResultList();
    }
}
```
```java
@Data
public class OrderSimpleQueryDto {
	private Long orderId;
	private String name;
	private LocalDateTime orderDate;
	private OrderStatus orderStatus;
	private Address address;
	
	public OrderSimpleQueryDto(Long orderId, String name, LocalDateTime orderDate, OrderStatus orderStatus, Address address) {
		this.orderId = orderId;
		this.name = name;
		this.orderDate = orderDate;
		this.orderStatus = orderStatus;
		this.address = address;
    }
}
```
* `createQuery()` 메서드에서 SELECT 절에서 `new`명령어로 DTO 객체로 즉시 변환 가능하다.
* 이 떄 DTO 객체의 생성자에는 조회하려는 값들을 하나하나 매개 변수로 받아야 한다. (엔티티 통째로 받진 못함)
  * 사실 Repository 엔티티의 객체 그래프를 조회하고 사용하는 데 쓰이는 용도로써 논리적인 성격으로는 맞지 않다.
    * DTO로 조회하면 API 스펙에 맞춰서 이미 쿼리가 짜여진 것이기 때문
    * Repository 메서드 재사용성 관점에서 볼 때도 DTO를 바로 조회하는 함수는 해당 DTO를 조회할 때만 사용하므로 재사용성이 떨어진다.
  * 또한 JPA에서 DTO를 바로 조회한다고 해도 성능 차이가 크게 나진 않는다.
    * SELECT 쿼리에서 컬럼 몇 개의 차이일 뿐

## 쿼리 방식 선택 권장 순서
1. 우선적으로 엔티티를 DTO 객체로 변환하는 방법을 선택하는 걸 권장한다.
2. 필요하면 패치 조인으로 성능을 최적화한다. (패치 조인으로 대부분의 성능 이슈가 해결된다.)
3. 그래도 성능 최적화를 해야한다면 DTO를 직접 조회하는 방법을 고려한다.
4. 최후의 방법은 JPA의 네이티브 SQL이나 스프링 JDBC Template를 사용해서 SQL을 직접 사용하는 방법을 선택한다.

