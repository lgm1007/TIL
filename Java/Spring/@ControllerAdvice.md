# @ControllerAdvice
* `@ControllerAdvice`는 주로 예외 처리와 관련된 기능을 하는 어노테이션으로, **모든 `@Controller` (전역)에서 발생할 수 있는 예외**를 잡아 처리해주는 어노테이션이다.
* Spring AOP의 원리를 활용하여 동작하지만, Spring의 예외 처리를 분리하고 중앙 집중화하는 새로운 개념이다.
### @ControllerAdvice 사용처
1. **전역 예외 처리**
    * Spring에서 예외가 발생했을 때, 기본적으로 `500 Internal Server Error`로 응답하게 된다.
    * 이러한 예외를 커스터마이징하여 클라이언트에게 더 나은 응답을 제공하거나 로그를 남기는 등의 작업을 할 수 있다.
2. **여러 컨트롤러에서 공통으로 처리해야 하는 예외**
    * 여러 컨트롤러에서 흩어져 있는 예외 처리 로직을 `@ControllerAdvice`를 사용하여 중앙 집중화할 수 있다.
3. **전역 모델 속성 추가**
    * 특정 모델 속성을 모든 컨트롤러에서 자동으로 추가하고 싶을 때 사용할 수 있다. 
4. **전역 바인딩 설정**
    * 요청 바인딩을 커스터마이징하거나, 특정 데이터 타입을 변환할 때 사용할 수 있다.
### @ControllerAdvice 개념 및 예제
1. `@ControllerAdvice` 클래스 생성
```java
@ControllerAdvice
public class GlobalExceptionHandler {
	
	// 예외 처리 메서드
    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception e) {
		return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body("서버에서 오류가 발생했습니다.");
    }
}
```

위 예제에서 `GlobalExceptionHandler` 클래스는 `@ControllerAdvice` 어노테이션이 적용되어 있다. 이 클래스는 전역 에외 처리를 담당한다. `@ExceptionHandler` 어노테이션을 통해 특정 예외를 처리할 수 있는 메서드를 정의한다.

위 예제에서는 모든 예외를 처리하는 `handleException` 메서드를 정의했다. 예외가 발생하면 `500 Internal Server Error`로 응답을 주는 대신, "서버에서 오류가 발생했습니다."라는 메시지와 함께 500 상태코드를 반환한다.

2. 예외를 발생시키는 컨트롤러
```java
@RestController
public class SampleController {
	
	@GetMapping("/hello")
    public String hello() {
		throw new RuntimeException("예외 발생!");
    }
}
```

`/hello`로 요청이 들어오면 의도적으로 예외를 발생시키는 코드가 작성되어 있다.

3. 실행 결과
    * 요청: http://localhost:8080/hello
    * 응답: 서버에서 오류가 발생했습니다. (`500 Internal Server Error`)

* `@ControllerAdvice`를 사용하여 전역적인 예외 처리가 적용되어 500 상태코드와 함께 메시지가 반환되는 것을 확인할 수 있다.
* 이처럼 `@ControllerAdvice`를 활용하면 여러 컨트롤러에서 발생하는 예외를 중앙에서 통합적으로 처리할 수 있으며, 유연하고 효율적인 예외 처리를 구현할 수 있다.

