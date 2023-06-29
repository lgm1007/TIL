# @Transient
* 엔티티 객체의 데이터와 DB 테이블의 컬럼과 매핑 관계를 제외하기 위해 사용하는 어노테이션
### 이해
```java
package javax.persistence;
@Target({ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Transient {}
```
* `@Transient`는 메서드와 필드에 선언할 수 있다.
* `@Entity` 클래스 뿐 아니라 `@MappedSuperclass`, `@Embeddable` 클래스의 필드나 `getter` 메서드에 선언할 수 있다.
#### 영속 대상에서 제외
* "컬럼을 제외한다." 라는 표현보다는 "**영속 대상에서 제외한다.**" 라고 보는 게 더 정확하다.
* JPA에선 영속성 컨텍스트라는 논리적인 패러다임 구현체인 **엔티티 매니저**가 존재하고, 해당 엔티티 매니저에서 `@Entity` 객체를 관리하게 된다.
* 영속 상태인 엔티티 객체는 엔티티 매니저에 의해 **변화에 대한 자동 감지(Dirty Checking)**,  **CRUD SQL 자동 생성 작업** 및 그 외 모든 JPA 내부적인 동작 프로세스에서 활용된다.
  * 하지만 영속 대상에서 제외되면, 더 이상 엔티티 매니저의 관리 대상이 아니라는 의미
  * 즉 `@Transient`를 선언하게 되면 앞서 설명한 작업들을 수행하지 않는다.
### 사용법
* `@Transient` 어노테이션은 데이터베이스와 직접적인 매핑이 필요하지 않은 임시로 사용되는 필드나 계산에 사용되는 필드 등을 표시할 때 유용하다.
* 예제 소스
```java
@Entity
public class Product {
	@Id
    private Long id;
	
	private String name;
	
	@Transient
    private BigDecimal discountedPrice;
	
	// etc.
    
    // 계산된 discountPrice 값을 반환하는 메서드
    public BigDecimal getDiscountedPrice() {
        // 계산 로직
        // return discountedPrice;
    }
}
```
* `discountedPrice` 필드는 데이터베이스에 매핑되지 않고 `getDiscountedPrice()` 메서드를 통해 계산된 값을 반환한다.
