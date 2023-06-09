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
   * 컬렉션 내 인자들 중 **주어진 조건과 일치하는 인자만 걸러**주는 함수
```kotlin
fun main(args: Array<String>) {
    val cities = listOf("Seoul", "Tokyo", "Beijing")
   
   cities.filter { city -> city.length == 5 }
           .forEach { println(it) }   // Seoul, Tokyo
}
```
2. **take()**
   * take()
     * 컬렉션 내 인자들 중 앞에서 `take()` 함수의 **인자로 받은 개수만큼**을 인자로 갖는 리스트를 반환하는 함수
   * takeLast()
     * `take()` 함수와 반대로 뒤에서부터 적용해 반환하는 함수
   * takeWhile()
     * **첫 번째 인자부터 시작**하여 **주어진 조건을 만족하는 인자까지를 포함하는** 리스트를 반환하는 함수
   * takeLastWhile()
     * `takeWhile()` 함수와 반대로 뒤에서부터 적용해 반환하는 함수
```kotlin
fun main(args: Array<String>) {
    val cities = listOf("Seoul", "Tokyo", "Beijing", "NYC", "London")
   
   cities.take(2)
           .forEach { println(it) }     // Seoul, Tokyo
   
   cities.takeLast(3)
           .forEach { println(it) }     // Beijing, NYC, London
   
   cities.takeWhile { city -> city.length <= 5 }
           .forEach { println(it) }     // Seoul, Tokyo, NYC
   
   cities.takeLastWhile { city -> city.length > 5 }
           .forEach { println(it) }     // London, Beijing
}
```
3. **drop()**
   * `take()` 함수의 반대 역할을 하여, 조건을 만족하는 항목을 컬렉션에서 제외한 결과를 반환하는 함수
```kotlin
fun main(args: Array<String>) {
    val cities = listOf("Seoul", "Tokyo", "Beijing", "NYC", "London")
   
   cities.drop(2)
           .forEach { printl(it) }      // Beijing, NYC, London
   
   cities.dropLast(3)
           .forEach { println(it) }     // Tokyo, Seoul
   
   cities.dropWhile { city -> city.length <= 5 }
           .forEach { println(it) }     // Beijing, London
   
   cities.dropLastWhile { city -> city.length > 5 }
           .forEach { println(it) }     // NYC, Tokyo, Seoul
}
```
4. **first(), last()**
   * 컬렉션 내 첫 번째 인자를 반환하는 함수
   * 단순히 리스트 내 첫 번째 인자를 반환하는 것 뿐 아니라, 특정 조건을 만족하는 첫 번째 인자를 반환하는 것도 가능하다.
     * 조건을 만족하는 인자가 없을 경우 `NoSuchElementException` 예외를 발생시키며, `firstOrNull()` 함수를 사용하면 Null 값을 반환하도록 할 수도 있다.
```kotlin
fun main(args: Array<String>) {
    val cities = listOf("Seoul", "Tokyo", "Beijing", "NYC", "London")
   
   println(cities.first())  // Seoul
   
   println(cities.last())   // London
   
   println(cities.first { it.length > 5 })  // Beijing
   
   println(cities.firstOrNull { it.length > 10 })   // Null
}
```
5. **distinct()**
   * 컬렉션 내에 포함된 항목 중 중복된 항목을 걸러낸 결과를 반환하는 함수
   * 항목의 중복 여부는 `equals()`로 판단하며, `distinctBy()` 함수를 사용하면 비교에 사용할 키 값을 직접 설정할 수 있다.
```kotlin
fun main(args: Array<String>) {
    val cities = listOf("Seoul", "Tokyo", "Beijing", "Seoul", "Tokyo")
   
   cities.distinct()
           .forEach { println(it) }     // Seoul, Tokyo, Beijing
   
   cities.distinctBy { it.length }  // length를 판단 기준으로 사용하도록 한다.
           .forEach { println(it) } // Seoul, Beijing   (Tokyo는 Seoul과 같은 5글자로 중복 제거된 상태)
}
```
### 조합 또는 합계
1. **zip()**
   * **두 컬렉션을 조합**하여 새로운 컬렉션을 만들어주는 함수
```kotlin
fun main(args: Array<String>) {
    val names = listOf("Kim", "Lee", "Park", "Choi")
    val age = lsitOf(32, 28, 30, 36)
   
   names.zip(age)
           .forEach { println(it) } // (Kim, 32) (Lee, 28) (Park, 30) (Choi, 36)
   
   names.zip(age) { name, age -> "$name 는 $age 살입니다." }
           .forEach { println(it) } // Kim 는 32 살입니다. Lee 는 28 살입니다. Park 는 30 살입니다. Choi 는 36 살입니다.
}
```
2. **joinToString()**
   * 컬렉션을 **문자열로 변환**하여 한 문자열로 반환한다.
```kotlin
fun main(args: Array<String>) {
    val names = listOf("Kim", "Lee", "Park", "Choi")
   
   println(names.joinToString())     // Kim, Lee, Park, Choi
   
   println(names.joinToString(" "))  // Kim Lee Park Choi
   
   println(names.joinToString { it -> "$it 입니다." })  // Kim 입니다., Lee 입니다., Park 입니다., Choi 입니다.
}
```
3. **count()**
   * 컬렉션 내 포함된 **자료의 개수**를 반환하는 함수
```kotlin
fun main(args: Array<String>) {
    val names = listOf("Kim", "Lee", "Park", "Choi")
   
   println(names.count())    // 4
   
   println(names.count { name -> name.length > 3 })  // 2
}
```
4. **reduce()**
   * 컬렉션 내 자료들을 **모두 합쳐 하나의 값**으로 만들어주는 함수
```kotlin
fun main(args: Array<String>) {
    val numbers = listOf(1, 2, 3, 4, 5)
    val strList = listOf("a", "b", "c")
   
   println(names.reduce { acc, s -> acc + s })  // 15  <- (1 + 2 + 3 + 4 + 5 를 한 값)
   
   println(strList.reduce { acc, str -> acc + str })    // abc
}
```
5. **fold()**
   * 컬력션 내 자료들을 **모두 합쳐 하나의 값**으로 만들어주면서 **초기값을 지정할 수 있는** 함수
```kotlin
fun main(args: Array<String>) {
    val numbers = listOf(1, 2, 3, 4, 5)
    val strList = listOf("a", "b", "c")
   
   println(numbers.fold(20) { acc, s -> acc + s })  // 35  <- (20 + 1 + 2 + 3 + 4 + 5)

   println(strList.fold("this is ") { acc, str -> acc + str })    // this is abc
}
```
### 부가 기능
1. **any()**
2. **none()**
