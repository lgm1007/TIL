# Kotlin 기초 문법
### 1. 변수 선언
* 변수를 선언할 때는 `var` 또는 `val` 키워드를 사용한다.
  * `val` 키워드는 변경할 수 없는 변수
  * `var` 키워드는 변겨할 수 있는 변수
* 변수를 초기화할 때 변수 타입을 생략할 수 있다.
* 변수 타입을 생략하는 경우 변수에 할당된 값을 기준으로 자동으로 타입이 지정된다.
```kotlin
// Int 타입으로 자동 지정
var a = 10
// String 타입으로 지정
var b: String = "Hello Kotlin"
```
### 2. 조건문
* 코틀린은 자바와는 달리 if문이 표현식으로 사용될 수 있다.
  * if문의 결과를 변수에 할당할 수 있다.
```kotlin
val x = 10
val y = 20
val  max = if (x > y) x else y
```
### 3. 반복문
* for문과 while문을 사용하여 반복문을 처리할 수 있다.
* for문 예제
```kotlin
val array = arrayOf(1, 2, 3, 4, 5)
for (i in array) {
    println(i)
}
```
* while문 예제
```kotlin
var i = 0
while (i < 10) {
    println(i)
	i++
}
```
### 4. 함수 선언
* 코틀린에서 함수 선언 시 `fun` 키워드를 사용한다.
* 함수를 호출할 때는 함수 이름 뒤에 괄호를 붙인다.
* 함수의 매개변수는 괄호 안에 선언하며, 반환값은 함수 선언문 뒤에 반환 타입으로 지정한다.
```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}

// 함수 호출
val result = sum(10, 20)
```
### 5. 클래스 선언
* 코틀린에서 클래스 선언 시 `class` 키워드를 사용한다.
* 클래스 내부에는 프로퍼티와 메서드를 선언할 수 있다.

```kotlin
class Person(val name: String, var age: Int) {
    fun introduce() {
        println("Hello! my name is $name and I'm $age years old.")
    }
}

// 클래스 인스턴스 생성
val person = Person("Brown", 30)

// 클래스 내부 메서드 호출
person.introduce()
```

