# Optional
* Optional은 자바 8에서 추가된 클래스로, Null-safe한 프로그래밍을 지원하는 클래스이다.
### Optional의 기능
* 기존 Java 에서는 null 처리를 위해 null 체크를 자주 해줘야 했지만, Optional을 사용하면 이를 간단히 처리할 수 있다.
* Optional 클래스는 null일 수 있는 객체를 감싸는 Wrapper 클래스이며, 해당 객체가 null이면 `empty`, null이 아니면 해당 객체를 담은 Optional 객체를 반환한다.
### Optional의 주요 메서드
* `Optional.of(value)`
    * value가 null이 아니면 value를 감싼 Optional 객체를 반환한다.
    * value가 null 이면 **NullPointerException** 예외가 발생한다.
* `Optional.ofNullable(value)`
    * value가 null이 아니면 value를 감싼 Optional 객체를 반환한다.
    * value가 null 이면 **empty Optional** 객체를 반환한다.
* `Optional.empty()`
    * 비어 있는 Optional 객체를 반환한다.
* `get()`
    * Optional 객체 안에 있는 값을 반환한다.
    * 값이 없는 경우 **NoSuchElementException** 예외가 발생한다.
* `isPresent()`
    * Optional 객체가 **비어있는지 여부**를 확인한다.
* `ifPresent(Consumer<T> consumer)`
    * Optional 객체 안에 값이 존재하는 경우 **해당 값을 인자로 받아 처리하는 Consumer를 실행**한다.
* `orElse(T other)`
    * Optional 객체 안에 값이 존재하는 경우 해당 값을 반환한다.
    * 그렇지 않은 경우 **인자로 전달된 기본값(other)을** 반환한다.
* `orElseGet(Supplier<? extends T> supplier)`
    * Optional 객체 안에 값이 존재하는 경우 해당 값을 반환한다.
    * 그렇지 않은 경우 대체 값을 제공하기 위해 `supplier`의 `get()` 메서드를 호출하여 대체 값을 반환한다.
* `map(Function<T, R> mapper)`
    * Optional 객체 안에 있는 값을 인자로 받아 처리하는 **함수(mapper)를 적용하고, 처리한 결과**를 Optional 객체로 반환한다.
* `flatMap(Function<T, R> mapper)`
    * Optional 객체 안에 있는 값을 인자로 받아 처리하는 함수(mapper)를 적용하고, 처리한 결과를 Optional 객체로 반환한다.
    * 이 때 mapper의 결과도 Optional 객체이다.
### Optional 사용 예제
```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        String inputString = "Hello World";
        
        Optional<String> optionalString = Optional.ofNullable(inputString);
        
        optionalString.map(String::length)
                .filter(len -> len <= 5)
                .ifPresentOrElse(
                        System.out.println,
                        () -> System.out.println("Too long")
                );
    }
}
```
* 문자열을 입력으로 받아 문자열의 길이가 5 이하인 경우 해당 문자열을 출력하고, 그렇지 않은 경우에는 "Too long" 이라는 메시지를 출력하는 예제 소스
### `orElse()`와 `orElseGet()` 차이점
* 보통 `orElse()`는 **null 여부와 상관없이** 동작하고, `orElseGet()`은 **null 일 때에만** 동작한다고 알려져 있다.
* Optional 클래스의 내부에서 메서드를 살펴보면 다음과 같다.
```java
return value != null ? value : other;   // orElse(T other)
return value != null ? value : other.get()  // orElseGet(Supplier<? extends T> supplier)
```
* `orElseGet(Supplier<? extends T> supplier)` 메서드는 value가 null 이면 `other`을 바로 실행하지 않고 `other.get()`을 실행한다.
  * 따라서 null이 아닐 때는 실행되지 않는 것
```java
public T orElse(getAnyName()) {
    return value != null ? value : getAnyName();
}

public String getAnyName() {
    return "anyName";
}
```
* 만약 위와 같이 `orElse(T other)` 에서 other이 메서드일 경우를 보면
  * `getAnyName()` 메서드가 반환하는 문자열인 "anyName" 값이 `T` 제네릭 변수에게 필요하기 때문에 우선 `getAnyName()` 메서드를 실행하게 된다.
  * 그래서 null 여부와 상관없이 동작한다는 의미
