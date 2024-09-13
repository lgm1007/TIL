# JPA 임베디드 타입
### 임베디드 타입 (복합 값 타입)
* 새로운 값 타입을 직접 정의해서 사용할 수 있는 개념으로, JPA에서 임베디드 타입이라고 한다.
* 주로 기본 값 타입을 모아 만들어서 복합 값 타입이라고 하며, int나 String과 같은 값 타입을 의미한다.

#### 임베디드 타입을 사용하지 않았을 때와 비교
* 임베디드 타입을 사용하지 않았을 떄
```java
@Entity
public class Member {
	@Id 
	@GeneratedValue
	private Long id;
	private String name;

	// 집 주소 표현
	private String city;
	private String street;
	private String zipcode;
}
```


* 임베디드 타입을 사용할 때
```java
@Entity
public class Member {
	@Id
	@GeneratedValue
	private Long id;
	private String name;

	@Embedded
	private Address homeAddress;
}
```
```java
@NoArgsConstructor       // 임베디드 타입은 기본 생성자 필수
@AllArgsConstructor
@Embeddable
public class Address {
	@Column(name="city")  // 매핑할 컬럼 정의 가능
	private String city;
	private String street;
	private String zipcode;

	// ...
}
```

### 임베디드 타입 사용하는 방법
* `@Embedded` : 값 타입을 사용하는 곳에 선언
* `@Embeddable` : 값 타입을 정의하는 곳에 선언

### 임베디드 타입의 장점
* 재사용성이 높아진다.
	* 정의한 임베디드 타입은 다른 엔티티에서 사용 가능하다.
* 임베디드 타입 내에서 의미 있는 메서드를 만들 수 있다.
* 객체와 테이블을 세밀하게 매핑하는 것이 가능하다.

### 임베디드 타입 특징
* 임베디드 타입은 값 타입과 마찬가지로, 값 타입을 소유한 엔티티의 생명주기에 의존한다.
	* 엔티티가 생성될 때 임베디드 타입에 값이 생성되고, 엔티티 사용이 끝났을 때 함께 끝난다.
* 임베디드 타입은 값 타입일 뿐, 임베디드 타입을 사용하기 전과 후에 매핑하는 테이블에는 변화가 없다.
* 임베디드 타입이 null 이면, 임베디드 타입 내 필드들이 모두 null 이 되고, 그와 매핑되는 컬럼값 또한 모두 null 이 된다.
* 임베디드 타입 클래스 안에 엔티티를 넣어 사용할 수도 있다.
```java
@Entity
public class Member {
	@Embedded
	private Address homeAddress;

	@Embedded
	private Period workPeriod;
}

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Embeddable
public class Period {
	private LocalDateTime startDate;
	private LocalDateTime endDate;

	private WorkHistory workHistory;   // 임베디드 타입에 Entity 사용

	public boolean isWork() {
		// ...
	}
}
```
```java
@Entity
public class WorkHistory {
	@Id
	// ...
}
```

