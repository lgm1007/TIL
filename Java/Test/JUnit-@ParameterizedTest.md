# @ParameterizedTest
### 개요
* 테스트 코드를 작성하다 보면, 한 개의 메서드에 대해서 여러 개의 테스트를 수행해야 하는 경우가 발생한다.
* 규칙이 많아지는 경우나 테스트하고 싶은 값이 많은 경우 일일이 테스트 코드를 작성하기에는 힘들 수 있다.
* JUnit에서는 여러 개의 테스트를 한 번에 작성하기 위한 @ParameterizedTest 라는 어노테이션을 제공한다.
  * @Test 대신 @ParameterizedTest 어노테이션을 선언해주고, 파라미터로 넘겨줄 값들을 지정해주면 되는데, 파라미터를 넘겨주는 어노테이션이 여러 종류가 있다.

### @ValueSource
* 하나의 테스트에 하나의 인수를 넘겨주는 방법
* 테스트에 주입할 값을 어노테이션에 배열로 지정한다.
* 테스트를 실행하면 배열을 순회하면서, 테스트 메서드에 인수로 배열에 저장된 값들을 주입하여 테스트한다.
* `@ValueSource`에 사용할 수 있는 자료형
  * byte, short, int, long, float, double, char, boolean
  * String, Class

```java
@ParameterizedTest
@ValueSource(strings = {"", " "})
void 유저네임으로_User_생성하기(String userName) {
    assertThatThrownBy(() -> new User(userName))
        .isInstanceOf(IllegalArgumentException.class);
}
```

### @CsvSource
* input 값과 expect (기대값) 값을 인수로 받고 싶은 경우 사용한다.
* value 매개변수로 값을 넘겨주는데, 이 때 "{input},{expect}"의 형태로 구분자가 있는 문자열로 입력을 넣어준다.
  * 기본 구분자는 콤마(',')이지만, delimiter 값을 직접 정의해서 커스텀 구분자를 사용할 수도 있다.
  * delimiter 구분자는 String이 아닌 char 값
  * ```java
     @CsvSource(value = {"1:2", "3:4"}, delimiter = ':')
    ```
  * String을 구분자로 넣고 싶다면 delimiterString 매개변수를 이용해 값을 넘겨줘야 한다.
  * ```java
     @CsvSource(value = {"1//2", "3//4"}, delimiterString = "//")
    ```

```java
@ParameterizedTest
@CsvSource(value = {"1,2", "2,4", "3,6"})
void 2_곱하기_테스트(int input, int expected) {
    assertThat(multiplyBy2(input)).isEqualTo(expected);
}
```

### @NullSource, @EmptySource, @NullAndEmptySource
* `@NullSource`는 테스트 인자로 null을, `@EmptySource`는 빈 값을, `@NullAndEmptySource`는 null과 빈 값을 모두 주입한다.
* 이 때 원시 값 (byte, short, int, long, float, double, char, boolean) 은 null이 들어갈 수 없으므로 메서드의 인자가 원시 값이라면 `@NullSource`, `@NullAndEmptySource`는 사용 불가능하다.
* `@NullSource`와 `@EmptySource`를 둘 다 사용한 것은 `@NullAndEmptySource`와 같다.
* `@ValueSource`와 같이 사용 가능하다.

```java
@ParameterizedTest
@NullAndEmptySource
@ValueSource(strings = {"", " "})
void 유저네임으로_User_생성하기(String name) {
    assertThatThrownBy(() -> new User(name))
        .isInstanceOf(IllegalArgumentException.class)
}
```

### @EnumSource
* Enum 클래스의 모든 값을 사용하려면 `@EnumSource` 안에 Enum 클래스만 전달해주면 되고, 특정 값만 필요할 경우 value에 Enum 클래스를 넣어주고, names에 선택할 값의 이름을 전달해주면 된다.
* 이 때 names까지 값을 넣으면 추가로 mode 값을 넣어줄 수 있다.
* 지원하는 mode는 다음과 같다.
  * INCLUDE
  * EXCLUDE
  * MATCH_ALL
  * MATCH_ANY

```java
@ParameterizedTest
@EnumSource(value = Color.class, names = {"PINK", "CORAL"})
void 색상의_베이스_색상_테스트(Color color) {
    assertThat(color.getBaseColor()).isEqualTo(Color.RED);
}
```

### @MethodSource
* 위 어노테이션들은 복잡한 Object를 전달하는 것은 불가능하다.
* `@MethodSource`는 메서드를 인수로 전달해주면 복잡한 인수를 전달하는 것이 가능하다.

```java
@ParameterizedTest
@MethodSource("provideBlankStrings")
void 문자열_isBlank_테스트(String input, boolean expected) {
    assertThat(input.isBlank()).isEqualTo(expected);
}

private static Stream<Arguments> provideBlankStrings() {
    return Stream.of(
        Arguments.of("", true),
        Arguments.of(" ", true),
        Arguments.of("not blank", false)
    );
}
```

* `provideBlankStrings()`라는 메서드를 정의한 뒤, 해당 메서드를 `@MethodSource`로 테스트 메서드에 넘겨주는 방식
* `@MethodSource`에 작성하는 메서드 이름은 인수로 제공하려는 메서드 이름과 같아야 한다.
* 인수로 전달하려는 메서드는 **`static` 메서드여야 한다.**
* `@MethodSource`에 메서드 이름을 작성해주지 않으면 JUnit이 테스트 메서드 네임과 같은 메서드를 찾아 인수로 제공한다.
