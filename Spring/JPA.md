# JPA
### ORM (Object-Relational Mapping)
* 애플리케이션의 Class와 Relational Database의 테이블을 매핑한다는 뜻
* 기술적으로는 애플리케이션의 객체를 RDB 테이블에 자동으로 **영속화**해주는 것
* **장점**
    1. SQL문이 아닌 메서드를 통해 DB를 조작할 수 있어 객체 모델을 이용하여 비즈니스 로직을 구성하는 데만 집중할 수 있다.
    2. Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어들어 각종 객체에 대한 코드를 별도로 작성하여 코드의 가독성을 높인다.
    3. 객체 지향적인 코드 작성이 가능하다.
    4. ORM을 사용한다면 Mysql에서 NoSQL과 같은 다른 데이터베이스로 변환할 경우 따로 쿼리를 수정할 필요가 없다.
* **단점**
    1. 프로젝트 규모가 크고 복잡하여 설계가 잘못될 경우, 속도 저하 등 일관성을 무너뜨리는 문제점이 생길 수 있다.
    2. 복잡하고 무거운 Query는 속도를 위해 별도 튜닝이 필요 -> 결국 SQL문을 써야 할 수도 있다.
    3. 학습 비용이 비싸다.
### JPA (Java Persistence API)
* Java 진영에서 ORM 기술 표준으로 사용하는 인터페이스 모음
* 자바 애플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스
* 인터페이스이기 때문에 Hibernate, OpenJPA 등이 JPA를 구성
### JPA 사용법 (JpaRepository)
#### Entity Mapping
* JPA를 사용하기 전에 가장 먼저 해야 할 일은 Entity와 Table을 정확하게 매핑하는 것
* JPA 매핑 어노테이션은 크게 4가지로 분류할 수 있다.
	1. 객체(Entity) ↔ 테이블(Table): `@Entity`, `@Table`
    2. 기본 키(PK; Primary Key): `@Id`
    3. 필드(Field) ↔ 컬럼(Column): `@Column` 
	4. 연관관계(FK; Foreign Key): `@ManyToOne`, `@ManyToMany`, `@OneToMany`, `@OneToOne`, `@JoinColumn`
##### @Entity
* `@Entity` 어노테이션이 붙은 클래스는 JPA에서 관리하게 된다.
* **제한사항**
	* 접근제한자가 `public` 또는 `protected` 인 기본 생성자가 반드시 필요하다.
    * `final`, `enum`, `interface`, `inner` 클래스에는 사용할 수 없다.
    * 저장할 필드에 `final`을 사용할 수 없다.

|속성|기능|기본값|
|---|---|---|
|name|JPA에서 사용할 엔티티의 이름을 지정 <br/> 이후 JPQL등에서 해당 이름을 사용 <br/> 프로젝트 전체에서 고유한 이름을 설정해야만 충돌 문제가 발생하지 않음|클래스 이름 그대로 사용|

##### @Table
* 엔티티와 매핑할 DB 테이블을 지정
* 생략하면 엔티티 이름을 그대로 사용

|속성|기능|기본값|
|---|---|---|
|name|엔티티와 매핑할 테이블 이름을 지정|엔티티 이름 그대로 사용|
|catalog|catalog 기능이 있는 DB에서 catalog 매핑|-|
|schema|schema 기능이 있는 DB에서 schema 매핑|-|
|uniqueConstraints|DDL 생성 시 복합 유니크 인덱스 생성 <br/> 뒤에 나올 `@Column`에 더 좋은 기능을 제공하기에 잘 사용하진 않음 <br/> 설정파일의 *DDL 옵션*에 따라 동작|-|

* 설정파일의 *DDL 옵션*
	* `create` : 애플리케이션 시작 시 모든 스키마를 드랍 후 새로 생성
    * `create-drop` : 애플리케이션 시작 시 모든 스키마를 드랍 후 생성, 애플리케이션 종료 시 모든 스키마 드랍
    * `update` : 애플리케이션 시작 시 기존 스키마에서 변경된 내역 적용
    * `validate` : 애플리케이션 시작 시 스키마가 제대로 매핑되는지 유효성 검사만 진행, 실패하면 애플리케이션 종료
    * `none` : JPA DDL 옵션 사용하지 않음
    * **주의** | **DDL 옵션은 항상 신중하게 확인해야 한다!**
        * 중요한 테이블이 통째로 드랍되거나 데이터가 삭제되는 사고가 발생할 수 있음
```yaml
# application.yaml
spring:
  jpa:
    hibernate:
      ddl-auto: none
```

##### @Id, @GeneratedValue
* 일반적으로 `@Id`를 붙인 필드는 테이블의 기본키(PK)와 매핑
	* 하지만, 반드시 물리적인 기본키와 매핑될 필요는 없다. (식별할 수 있는 값이기만 하면 된다.)
	* 영속성 컨텍스트에 저장되는 엔티티들이 `@Id`를 key로 하여 HashMap으로 저장되는 특징 때문인 것으로 보임
* `@Id` 지원 Java 타입
	* 자바 기본타입(primitive type) : int, long, float, double 등
    * 자바 래퍼타입(wrapper type) : Integer, Long 등
    * String
    * java.util.Date 또는 java.sql.Date
    * java.math.BigInteger
    * java.math.BigDecimal
    * **단, 실무에서는 일반적으로 primitive type은 사용하지 않는다.**
		* 오브젝트 타입으로 id를 사용하면 `nullable`하기 때문에 값을 초기화하지 않으면 null로 초기화되어 `int` 타입처럼 id 값이 0으로 초기화되는 경우를 방지해주기 때문
        * 대부분 `Long` 타입을 가장 많이 사용한다.
* `@GeneratedValue`를 사용하지 않을 경우 아래와 같이 기본키를 직접 할당해야 한다.
```java
Member member = new Member();
member.setId(1L);
memberRepository.save(member);
```

* `@GeneratedValue`를 사용할 경우는 다음과 같다.
```java
Member member = new Member();
memberRepository.save(member);
```

* `@GeneratedValue` 기본키 생성 전략
	* `@GeneratedValue(strategy = GenerationType.TABLE)`
	* `@GeneratedValue(strategy = GenerationType.SEQUENCE)`
	* `@GeneratedValue(strategy = GenerationType.IDENTITY)`
	* `@GeneratedValue(strategy = GenerationType.AUTO)`
* 보통 Mysql 또는 Mariadb 를 사용한다면 `IDENTITY`, Oracle 을 사용한다면 `SEQUENCE` 전략을 사용하는 편
* **주의** | `IDENTITY` 전략 사용 시 insert 쿼리에 대해 쓰기지연 기능이 제대로 동작하지 않을 수 있다. 

##### @Column
* 엔티티 필드와 테이블 컬럼을 매핑하는 데 사용

|속성|기능|기본값|
|---|---|---|
|name|필드와 매핑할 테이블의 컬럼명|객체의 필드명|
|insertable|엔티티 저장 시 해당 필드도 같이 저장 <br/> false로 설정 시 해당 필드는 DB에 저장하지 않음|true|
|updatable|엔티티 수정 시 해당 필드도 같이 수정 <br/> false로 설정 시 해당 필드는 DB에 수정하지 않음|true|
|table|하나의 엔티티를 두 개 이상의 테이블에 매핑할 때 사용|현재 클래스가 매핑된 테이블|
|nullable|null 허용 여부 <br/> false로 설정 시 DDL 옵션으로 인한 DDL 생성 시 컬럼에 not null 제약 조건이 붙음|true|
|unique|단일 유니크 인덱스를 생성할 때 사용 <br/> `@Table`의 uniqueConstraints 속성은 복합 유니크 인덱스를 생성할 때 사용함|-|
|columnDefinition|DB 컬럼 정보를 직접 입력 (Native)|-|
|length|문자 길이 제약 조건으로, String 에만 사용|255|
|precision|전체 자릿수(정밀도) 설정 <br/> BigInteger, BigDecimal 타입 사용 시|0|
|scale|소수점 이하 자리수 설정 <br/> BigInteger, BigDecimal 타입 사용 시|0|

##### Entity 클래스 예제
```java
@Entity
@Table(name = "service_member")
public class Member {
	@Id
	@Column(name = "id")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	
	@Column(name = "name")
	@NotNull
	private String name;    // not null
	
	@Column(name = "age")
	private Integer age;     // nullable
}
```

#### JpaRepository
* Entity(Domain) 클래스를 작성한 후 Repository 인터페이스를 생성
* Spring boot에서는 Entity의 기본적인 CRUD가 가능하도록 **JpaRepository 인터페이스**를 제공한다.
* ```java
   public interface MemberRepository extends JpaRepository<Member, Long> {  // JpaRepository<T, ID> (사용할 Entity 클래스와 ID (PK) 값)
   }
  ```
* JpaRepository 를 상속받는 것만으로 위의 정의한 인터페이스는 Entity 하나에 대하여 아래 기본적인 기능들을 제공하게 된다.  

|메서드|기능|
|---|---|
|save()|레코드 저장 (insert, update)|
|findOne()|PK로 레코드 한 건 찾기|
|findAll()|전체 레코드 불러오기. 정렬(sort), 페이징(pageable) 가능)|
|count()|레코드 개수|
|delete()|레코드 삭제|

#### Select
* 기본적인 기능을 제외한 조회 기능을 추가하고자 하면 **규칙에 맞는 메서드**를 추가해야 한다.  

|메서드|설명|
|---|---|
|findBy로 시작|조회를 요청하는 쿼리임을 알림|
|countBy로 시작|쿼리 결과 레코드의 개수를 요청하는 쿼리임을 알림|

* ```java
  public interface MemberRepository extends JpaRepository<Member, Long> {
    Member findByName(String name);

    Page<Member> findById(Long id);
  }
  ```
* 메서드의 반환형이 **Entity** 객체이면 하나의 결과만을 전달, **List** 이면 쿼리에 해당하는 모든 객체를 전달
* **쿼리 메서드에 포함할 수 있는 키워드**

|키워드|예제 코드|설명|
|---|---|---|
|And|findByEmailAndId(String email, Long id)|여러 필드를 and로 검색|
|Or|findByEmailOrId(String email, Long id)|여러 필드를 or로 검색|
|Between|findByUpdatedAtBetween(Date fromDate, Date toDate)|필드의 두 값 사이에 해당하는 항목 검색|
|LessThan|findByAgeLessThan(Integer age)|작은 항목 검색|
|GreaterThanEqual|findByAgeGreaterThanEqual(Integer age)|크거나 같은 항목 검색|
|Like|findByNameLike(String name)|like 검색|
|isNull|findByJobIsNull()|null인 항목 검색|
|In|findByJobIn(String job1, ..., String jobs)|여러 값 중 하나에 해당하는 항목 검색|
|OrderBy|findByNameOrderByAgeAsc(String name)|검색 결과를 정렬|

* 쿼리 메서드의 입력변수로 **Pageable** 객체를 추가하면 Page 타입을 반환형으로 사용할 수 있다.
* Pageable 입력 변수는 Controller 에서부터 전달받아야 한다.
* ```java
  public class MemberController {
      @RequestMapping("")
      Page<Member> getMembers(Pageable pageable) {
          return memberService.getList(pageable);
      }
  }
  ```
* 위와 같이 작성된 Pageable에서는 아래의 파라미터를 자동으로 수집한다.

|쿼리 파라미터명|설명|
|---|---|
|page|몇 번째 파라미터인지 전달|
|size|한 페이지에 몇 개의 항목을 보여줄지 전달|
|sort|정렬 정보를 전달. 정렬 정보는 필드이름, 정렬방향의 포맷으로 전달<br/>여러 필드로 순차적으로 정렬도 가능<br/>ex) sort=updatedAt,desc&sort=age,asc

#### Insert & Update
1. **Insert**
	* insert는 DB 내 데이터와 상관 없이 새로운 데이터를 생성하여 넣는 것
    * 따라서 영속성이 없어도 insert 쿼리를 실행할 수 있음
```java
public Ticket insert() {
	Ticket ticket = Ticket.builder()
		.key("123-456-7890")
		.build();
	
	return ticketRepository.save(ticket);
}
```
2. **Update**
	* update는 DB의 데이터를 변경하는 것이기에 영속성이 필수이다.
    * ```java
       // 잘못된 JPA update 예제
	     public Ticket update(Long id) {
           Ticket ticket = Ticket.builder()
              .id(id)
              .key("123-456-7890")
              .build();

           return ticketRepository.save(ticket);
        }
        ```  
	* 위 소스는 비영속성으로 DB와 연결중인 상태가 아니다.
    * 영속성을 유지하기 위해선 DB에서 조회를 해오면 유지된다. (1차 캐시)
    * ```java
       // 업데이트는 실행되는 소스이지만, 중복 조회 발생
       public Ticket update(Long id) {
           Ticket ticket = ticketRepository.findById(id);   // DB 조회
           ticket.setKey("123-456-7890");
           return ticketRepository.save(ticket);
       }
      ```
	* JPA에서 `save()`를 호출하면 insert인지 update인지 판단을 위해 findById 조회를 한 번 거친다.
    * 즉 위 소스는 중복 조회를 일으키게 된다.
    * 영속성이 유지되는 Entity는 Transaction이 종료되면 변경된 필드를 자동으로 감지하여 DB에 commit 해준다. (Dirty Checking)
	* ```java
      // 가장 효율적인 업데이트 메서드
      // Transaction 상태 설정 어노테이션
      @Transactional
      public Ticket update (Long id) {
          Ticket ticket = ticketRepository.findById(id);
          ticket.setKey("123-456-7890");
          return ticket;    // Transaction이 종료되는 시점에 변경된 상태 commit
      }
      ```

### JPA Mysql 컬럼 자료형
|Java|Mysql|
|---|---|
|Byte|tinyint(4)|
|Short|smallint(6)|
|Integer|int(11)|
|Long|bigint(20)|
|BigDecimal|decimal(19,2)|
|Float|float|
|Double|double|
|Boolean|bit(1)|
|Date <br/> LocalDate|date|
|Timestamp <br/> LocalDateTime|datetime|
|Time|time|
|String|varchar(255)|
|Clob|longtext|
|Byte[][2]|tinyblob|
|Blob|longblob|

