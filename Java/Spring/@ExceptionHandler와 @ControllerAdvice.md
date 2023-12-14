# @ExceptionHandler와 @ControllerAdvice
### @ExceptionHandler
* `@ExceptionHandler` 어노테이션은 `@Controller` 또는 `@RestController`가 적용된 Bean 내에서 발생하는 예외를 잡아 어노테이션이 정의된 메서드에서 처리해주는 기능을 해준다.
* `@Controller`, `@RestController` 가 선언된 클래스 내에서 사용 가능하며, `@Service`와 같은 컴포넌트 객체에서는 적용하지 못한다.
* 사용 예시
```java
@RestController
public class MyRestController {
    // ...
    
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity handleIllegalException(IllegalArgumentException e) {
        return new ResponseEntity(e.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```
```java
@Slf4j
@RestController
public class MyRestController {
    // ...
    
    // 동시에 여러 가지 예외 처리도 가능하다.
    @ExceptionHandler({RuntimeException.class, IOException.class})
    public void handledRuntimeAndIOException(Exception e) {
        log.error(e.getMessage, e);
    }
}
```

### @ControllerAdvice
* `@ExceptionHandler`를 사용할 때 불편한 점은 정상 코드와 예외 코드가 하나의 클래스에 정의되어 있다는 점이다.
* 이러한 불편한 점을 해결하기 위해 `@ControllerAdvice` 또는 `@RestControllerAdvice`를 사용한다.
* `@ControllerAdvice`는 특정 대상 또는 모든 글로벌한 컨트롤러에 대해 전역적으로 예외를 핸들링할 수 있게 해주는 어노테이션이다.
  * 즉 하나의 클래스에 정의되어 있는 정상 코드와 예외 코드를 분리하기 위해 사용하는 어노테이션이다.
* 사용 예시
```java
// 대상을 지정하지 않고 사용하면 모든 컨트롤러에 대해 적용된다.
@ControllerAdvice
@Order(1)
public class MyControllerAdvice {
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity handleIllegalException(IllegalArgumentException e) {
        return new ResponseEntity(e.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```
* **주의할 점**: `@ControllerAdvice`를 대상을 지정하지 않고 사용할 때 `@Order` 어노테이션으로 순서를 지정하지 않고 사용할 경우 Spring 에서는 임의의 순서로 호출하게 된다. 따라서 개발자가 예상하지 못한 예외가 발생할 수도 있다.

#### @ControllerAdvice 의 대상 지정
1. 특정 어노테이션이 있는 컨트롤러
```java
@ControllerAdvice(annotations = RestController.class)
public class MyControllerAdvice {}
```

2. 해당 패키지와 하위 패키지에서 발생하는 예외
```java
@ControllerAdvice(basePackages = {"org.example.controller"})
public class BasePackagesControllerAdvice {}
```

3. 특정 클래스
```java
@ControllerAdvice(assignableTypes = {SimpleController.class, ExampleController.class})
public class AssignableTypesControllerAdvice {}
```

#### @ControllerAdvice와 @RestControllerAdvice의 차이?
* `@Controller`와 `@RestController` 간의 차이와 같다고 보면 된다.
* `@ControllerAdvice`에서는 일반적으로 view를 반환한다.
* `@RestControllerAdvice` 에서는 `@ResponseBody`가 붙어있어 응답을 json 형식으로 객체 데이터를 내려준다.


