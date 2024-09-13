# 값 타입 컬렉션 (@ElementCollection, @CollectionTable)
### @ElementCollection
* 값 타입을 하나 이상 저장할 때 사용하는 어노테이션 (일대다)
* JPA 에서는 `@ElementCollection`을 이용해서 **Collection 객체**임을 JPA가 알 수 있게 설정할 수 있다.
* 데이터베이스에서는 컬렉션을 같은 테이블에 저장할 수 없어 컬렉션을 저장하기 위한 별도의 테이블이 필요하다.

### @CollectionTable
* 값 타입 컬렉션을 매핑할 테이블에 대한 역할을 지정하는 데 사용한다.
* 테이블의 이름과 조인 정보를 지정해줘야 한다.

### @ElementCollection VS 연관관계 (OneToMany, ManyToOne)
#### @ElementCollection
* 값 타입 컬렉션은 자신의 라이프사이클을 가지지 않는다.
* 부모 Entity에 의해 관리된다.
  * 생성된 컬렉션 테이블에는 부모의 ID와 관련된 키가 생성된다.
* 부모와 함께 저장되고 삭제된다. Cascade 옵션은 default
* 컬렉션 값 변경 시, 전체 삭제 후 새로 추가된다.

#### 연관관계 (OneToMany, ManyToOne)
* 여러 Entity와 연관관계를 가질 수 있다.
* 연관관계를 맺을 때 보통 ID를 사용한다.
* 실무에서는 상황에 따라 값 타입 컬렉션 대신 일대다 연관관계를 고려한다.

#### 사용 예시
##### 값 타입 하나 이상 저장 시
```java
@Entity
@Getter @Setter
public class Member {
    @Id @GeneratedValue
    @Column(name = "member_id")
    private Long id;
    
    private String name;
    
    @Embedded
    private Address address;
    
    @ElementCollection
    @CollectionTable(name = "address", joinColumns = @JoinColumn(name = "member_id"))
    private List<Address> addressHistory = new ArrayList<>();
}
```

##### 값 타입 저장
```java
member.getAddressHistory().add(new Address("old1", "street", "99-1"));
member.getAddressHistory().add(new Address("old2", "street", "100-1"));
```

##### 값 타입 수정
```java
findMember.getAddressHistory().remove(new Address("old1", "street", "99-1"));
findMember.getAddressHistory().add(new Address("new1", "street", "101-1"));
```

```java
public class Address {
    private String city;
    private String street;
    private String zipcode;
    
    @Override
    public boolean equals(Object o) {
        if (this == 0) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Address address = (Address) o;
        return Objects.equals(city, address.city) &&
                Objects.equals(street, address.street) &&
                Objects.equals(zipcode, address.zipcode);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(city, street, zipcode);
    }
}
```
* 값 타입 컬렉션으로 사용하는 객체는 반드시 `equals`와 `hashCode` 메서드를 필수 오버라이딩 해야 한다.
* 값 타입은 엔티티와 다르게 식별자 개념이 없다. 따라서 값을 변경하면 추적이 어렵다.

