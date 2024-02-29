# Cannot construct instance of ~~~
### 에러 상황
* DTO 객체를 잭슨의 Object Mapper로 직렬화/역직렬화할 떄 기본 생성자가 없을 경우 발생하는 에러
```
MismatchedInputException: Cannot construct instance of `~~~` (although at least one creator exists) ...
```

### 에러 원인
* Object Mapper에서 직렬화/역직렬화 바인딩을 할 때 기본 생성자를 사용하며, 기본 생성자가 존재하지 않으면 동작하지 않는다.
* 내부 코드 구성
```java
/*
/**********************************************************
/* Public API implementation; instantiation from JSON Object
/**********************************************************
 */

@Override
public Object createUsingDefault(DeserializationContext ctxt) throws IOException {
    // 기본 생성자가 null 인지 확인하는 부분
    if (_defaultCreator == null) {
        return super.createUsingDefault(ctxt);
    }
    try {
        return _defaultCreator.call();
    } catch (Exception e) {
        return ctxt.handleInstantiationProblem(_valueClass, null, rewrapCtorProblem(ctxt, e));
    }
}
```
* `if (_defaultCreator == null)` 부분에서 기본 생성자가 null인지 확인하고, null이면 `super.createUsingDefault(ctxt);` 메서드를 호출되며, 에러로그가 출력될 것이다.
* 기본 생성자가 있다면 호출하는 `_defaultCreator.call();` 메서드를 들어가보면 기본 생성자를 사용하는지 알 수 있다.
```java
@Override
public final Object call() throws Exception {
    return _constructor.newInstance(Object[]) null;
}
```

### 에러 관련 주의점
* lombok의 `@Builder`는 기본 생성자를 생성해주지 않는다.
* 기본 생성자의 접근자가 public이 아닐 경우에 constructor를 reflection API로 가져가 데이터를 주입하기 때문에 기본 생성자는 `private`로 만들어도 된다.
* `@JsonProperty`, `@JsonAutoDetect` 등의 어노테이션을 사용하는 프로퍼티 기반의 객체나 생성자를 위임한 경우엔 기본 생성자가 필요하지 않다.

