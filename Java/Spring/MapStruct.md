# MapStruct
* Entity와 Dto 객체 간의 매핑을 지원하는 라이브러리
  * Entity와 Dto 객체 간의 매핑을 지원하는 라이브러리에는 ModelMapper와 MapStruct가 있다.
  * MapStruct는 컴파일 시 미리 생성된 구현체로 매핑해주기 때문에 런타임 비용이 적게 들며 매핑 성능이 뛰어나다.
  * MapStruct는 **인터페이스 기반**으로 코드를 생성한다.
    * 각 매핑을 위한 메서드를 간단하게 정의하면 해당 메서드르 구현하는 코드를 생성한다.
### MapStruct의 주요 기능
1. 다양한 매핑 전략 및 기능 제공
2. 어노테이션 기반 설정
3. 다른 매핑 라이브러리와 통합 가능
4. Gradle 및 Maven 플러그인 제공
5. Lombok 과 통합 가능
### MapStruct 사용하기
#### 매핑 인터페이스 작성
* 인터페이스는 @Mapper 어노테이션을 사용하여 매핑 대상 클래스를 명시한다.
* @Mapping 어노테이션을 사용하면 매핑 시 특정 필드나 메서드를 지정할 수 있다.
#### 작성 예제
  * `Source` 클래스를 `Target` 클래스로 매핑하려면 아래와 같이 작성한다.
  * 인터페이스와 메서드 명은 원하는 대로 지정 가능
```java
@Mapper
public interface SourceTargetMapper {
    SourceTargetMapper INSTANCE = Mappers.getMapper(SourceTargetMapper.class);
    
    @Mapping(source = "id", target = "targetNo")
    Target sourceToTarget(Source source);
}
``` 
* 예제에서는 `INSTANCE`를 선언하여 클라이언트에 매퍼 구현체에 대한 엑세스를 제공한다.
  * `INSTANCE`를 선언하지 않는다면, 의존성 주입으로 `SourceTargetMapper` 구현체를 주입하여 사용할 수도 있다.
  * ```java
    @Autowired
    private SourceTargetMapper mapper;
    ```
* @Mapping 어노테이션으로 Source 객체의 `id` 라는 필드값을 Target 객체의 `targetNo` 필드값으로 매핑한다.
  * source와 target 대상이 다른 타입인 경우 형 변환을 실행하여 매핑한다.
#### 매핑 구현체 생성
* MapStruct는 컴파일 타임에 매핑 구현체를 생성해준다.
* `@Mapper(componentModel = "spring")` 와 같이 `componentModel="spring"` 속성을 지정해주게 되면 자동 생성되는 구현체 클래스를 스프링 빈으로 등록할 수 있다.
  * `componentModel` 속성을 생략하게 되어도 Spring 컴포넌트 모델을 사용하여 구현체를 생성하긴 한다.
#### 매핑 실행
```java
Target target = SourceTargetMapper.INSTANCE.sourceToTarget(source);
```
#### 작성 예제2
```java
@Mapper
public interface OrderMapper {
	@Mapping(source = "owner.name", target = "ownerName")
	@Mapping(source = "orderItems", target = "orderItems")
	@Mapping(source = "buyDate", target = "buyDate")   
	@Mapping(target = "totalPrice", qualifiedByName = "calculateTotalPrice")
    OrderDto mapToDto(Order order);
	
	@Named("calculateTotalPrice")
    default double calculateTotalPrice(Order order) {
		double total = 0.0;
		for (OrderItem item : order.getOrderItems()) {
			double itemPrice = item.getPrice();
			int quantity = item.getQuantity();
			total += itemPrice * quantity;
        }
		return total;
    }
}
```
* `calculateTotalPrice` 라는 사용자 정의 메서드를 정의한다.
* `@Mapping` 어노테이션의 `qualifiedByName` 속성에서 `@Named` 어노테이션으로 이름이 부여된 사용자 정의 메서드를 참조한다.
* 참조한 `calculateTotalPrice` 메서드에서 계산한 값을 `OrderDto` 객체의 `totalPrice` 필드에 매핑한다.
