# JPA 페치 조인 시 페이징과 성능 최적화 보장
## 페이징
* 컬렉션을 페치 조인하면 페이징이 불가능하다는 문제가 있다.
  * 컬렉션을 페치 조인하면 일대다 조인이 발생하므로 조회되는 데이터가 예측할 수 없이 증가한다.
  * 페이징을 한다면 일대다 조인에서 일(1)을 기준으로 페이징을 하는 것이 목적이다. 하지만 데이터는 다(N)를 기준으로 row가 생성된다.
* 이 경우 하이버네이트에서 경고 로그를 남기고 모든 DB 데이터를 읽어 와 메모리에서 페이징을 시도한다.
  * OOM (Out Of Memory) 장애가 발생할 수도 있다!

### 페이징과 컬렉션 엔티티를 함께 조회하는 방법은?
* 우선 `ToOne`(OneToOne, ManyToOne) 관계를 모두 페치조인한다.
  * `ToOne` 관계는 row수를 증가시키지 않으므로 페이징 쿼리에 영향을 주지 않는다.
* 컬렉션은 지연 로딩으로 조회한다.
* 지연 로딩 최적화를 위해 `hibernate.default_batch_fetch_size`, 또는 `@BatchSize`를 적용한다.
  * `hibernate.default_batch_fetch_size`: 글로벌 설정
  * `@BatchSize`: 개별 최적화
  * 위 옵션들을 설정하면 컬렉션이나 프록시 객체들을 한꺼번에 설정한 size 만큼 IN 쿼리로 조회한다.

#### hibernate.default_batch_fetch_size
* properties 설정에서 다음과 같이 설정을 추가한다.
```yaml
spring:
  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        default_batch_fetch_size: 100
```

#### @BatchSize
* 좀 더 디테일하게 설정하고 싶을 때 사용하는 어노테이션

1. 컬렉션 엔티티에는 필드에 직접 설정한다.
```java
@Entity
@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Order {
    @Id
    @GeneratedValue
    @Column(name = "order_id")
    private Long id;
    
    @ManyToOne(fetch = LAZY)
    @JoinColumn(name = "member_id")
    private Member member;
    
    @BatchSize(1000)
    @OneToMany(mappedBy = "order", cascade = CascadType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();
}
```
2. `ToOne`에 해당하는, 컬렉션이 아닌 엔티티의 경우 class 전체에 설정해준다.
```java
@BatchSize(100)
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

### `hibernate.default_batch_fetch_size`, `@BatchSize` 사용 시 장점
1. 쿼리 호출 수가 `1 + N`에서 `1 + 1`로 최적화된다.
2. 페치 조인 방식보다 쿼리 호출 수는 약간 증가하지만, **DB 데이터 전송량이 감소**한다.
3. 조인보다 DB 데이터 전송량이 최적화된다.
  * 조인으로 조회하면 중복 데이터가 조회될 수 있지만, BatchSize를 사용한 방법은 각각 조회하므로 중복 데이터가 없다.
4. 컬렉션 페치 조인은 페이징이 불가능하지만 BatchSize를 사용한 방법은 **페이징이 가능**하다.

## 추가 팁
* `ToOne` 관계는 페치 조인해도 페이징에 영향을 주지 않는다.
* 따라서 `ToOne` 관계는 페치 조인으로 쿼리 수를 줄이고, 나머지는 BatchSize를 사용하여 최적화하는 것이 좋은 방법이다.
