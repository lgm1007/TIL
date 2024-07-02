# Lombok @Builder 상속 시 cannot hide builder() 에러
### 에러 상황
* `@Builder`를 사용하면 컴파일 시 해당 클래스에 Builder 클래스와 `builder()` 메서드가 생성된다.
* 자식 노드에서도 동일하게 `@Builder`를 사용하면 상속 중인 부모 객체에서의 `builder()` 메서드가 중복되면서 에러가 발생하게 된다.

### 해결법
* 자식 객체 또는 부모 객체에서 `@Builder`의 property 중 하나인 `builderMethodName`을 설정해주면 된다.
* 물론 사용 시에는 `builderMethodName`에서 지정해 준 method 명을 사용해서 호출해야 한다.

```java
@Getter
public class ExampleResponse extends ParentResponse {
    private final int number;
    private final String message;
    private final String code;
    
    @Builder(builderMethodName = "exampleResponseBuilder")
    public ExampleResponse(final int number,
                           final String message,
                           final String code) {
        this.number = number;
        this.message = message;
        this.code = code;
    }
}
```
```java
// 실제 빌더 메서드 사용 시
ExampleResponse exampleRespone = ExampleResponse.exampleResponseBuilder()
    .number(number)
    .message(message)
    .code(code)
    .build();
```
