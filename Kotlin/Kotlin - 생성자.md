# Kotlin 생성자
### 1. 기본 생성자
* 클래스의 주요 생성자
* 클래스의 선언부에서 선언되는 게 특징
* 클래스의 프로퍼티를 선언하고 초기 값을 할당하는 역할 수행
* 기본 생성자 - 예제
```kotlin
class ExampleClass(val prop1: Int, val prop2: String) {
    // 클래스의 프로퍼티와 초기화 로직
}
```
### 2. 보조 생성자
* 클래스의 보조적인 생성자로 클래스 본문에서 선언되는 게 특징
* 클래스의 인스턴스를 생성할 때 사용자가 정의한 로직에 따라 초기화를 수행한다.
* 보조 생성자 - 예제
```kotlin
class ExampleClass {
    // 클래스의 프로퍼티 선언
    
    constructor(prop1: Int, prop2: String) {
        // 보조 생성자의 로직
    }
    
    constructor(prop1: Int) : this(prop1, "default") {
        // 보조 생성자의 로직
    }
}
```
* 보조 생성자는 `constructor` 키워드를 통해 선언된다.
* 클래스의 주요 생성자`(this)`를 호출하거나, 특정한 초기화 로직을 직접 작성할 수 있다.
* 보조 생성자는 인스턴스를 생성할 때 다양한 인자 조합을 허용하고, 다양한 초기화 로직을 구현할 수 있기 때문에 유연한 객체 생성을 지원한다.
### 전반적인 클래스 형태
```kotlin
class Person(val name: String, val age: Int) {
    // 주요 생성자
    
    // 보조 생성자 1
    // name과 0을 인자로 주 생성자 호출
    constructor(name: String) : this(name, 0) {
        // 보조 생성자의 로직
    }
    
    // 보조 생성자 2
    constructor(name: String, age: Int, address: String) : this(name, age) {
        // 보조 생성자의 로직
        // address를 사용한 초기화 로직
    }
    
    // 클래스의 메서드나 프로퍼티 등
    fun greet() {
        println("안녕하세요. 저는 $name이고 나이는 $age살입니다.")
    }
}
```
### 초기화
* 생성자의 파라미터로 받은 값의 유효성을 검사하고 초기화하는 경우 사용하는 방법
* `init` 블록
  * 코틀린에서는 주 생성자에 어떠한 코드도 추가될 수 없으며, 초기화 시 필요한 작업(유효성 검증 등)을 하기 위해 `init` 블록을 지원한다.
  * `init` 블록에는 클래스의 객체가 만들어질 때 실행될 초기화 코드가 들어간다.
  * 초기화 블록을 주로 **주 생성자**와 함께 사용한다.
```kotlin
class Class(val nameParam: String) {
    val name: String

    init {
        if (nameParam.isEmpty()) {
            throw IllegalArgumentException("Error")
        }
        this.name = nameParam
    }
}
```
