# Kotlin Stream
* 코틀린에서는 자바 8 이상에서의 스트림 대신 이와 유사한 역할을 하는 함수들을 **표준 라이브러리에서 제공**하며, 확장 함수 형태로 제공된다.
### 변환
1. **map()**
    * 컬렉션 내 인자들을 **다른 값 또는 다른 타입으로 변환**할 때 사용하는 함수
```kotlin
fun main(args: Array<String>) {
    val fruits = listOf("Apple", "Banana", "Citron")
    
    fruits.map { fruit -> fruit.toUpperCase() }
            .forEach { println(it) }    // APPLE, BANANA, CITRON
    
    fruits.map { fruit -> fruit.length }
            .forEach { println(it) }    // 5, 6, 6
}
```
2. **mapIndexed()**
    * 컬렉션 내 포함된 **인자의 인덱스 값을 변환 함수 내에서 사용**할 때 사용하는 함수
```kotlin
fun main(args: Array<String>) {
    // [0, 1, 2, 3, 4, 5]
    val numbers = 0..5
    
    numbers.mapIndexed { idx, number -> idx * number }  // 0*0, 1*1, 2*2, 3*3, 4*4, 5*5
            .forEach { println(it) }    // 0, 1, 4, 9, 16, 25
}
```
3. **mapNotNull()**
    * 컬렉션 내 인자를 **변환함과 동시에 변환한 결과가 Null 값인 경우** 이를 무시하는 함수
```kotlin
fun main(args: Array<String>) {
    val fruits = listOf("Apple", "Banana", "Citron")
    
    fruits.mapNotNull { fruit -> if (fruit.length <= 5) fruit else null }
            .forEach { println(it) }    // Apple
}
```
4. **groupBy()**
    * 컬렉션 내 인자들을 **지정한 기준에 따라 분류**하여 **맵 형태**로 결과를 반환하는 함수
```kotlin
fun main(args: Array<String>) {
    val fruits = listOf("Apple", "Banana", "Citron")
    
    fruits.groupBy { fruit -> if (fruit.length <= 5) "A" else "B" }
            .forEach { (key, fruits) -> println("$key $fruits") }   // A [Apple], B [Banana, Citron]
}
```
### 필터
1. **filter()**
2. **take()**
3. **drop()**
4. **first(), last()**
5. **distinct()**
### 조합 또는 합계
1. **zip()**
2. **joinToString()**
3. **count()**
4. **reduce()**
5. **fold()**
### 부가 기능
1. **any()**
2. **none()**
