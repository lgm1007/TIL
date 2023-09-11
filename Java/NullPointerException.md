# java.lang.NullPointerException
### Null 값
* 아무것도 없는 값을 의미
  * 0이나 공백("")과는 다른 개념
* 포인터가 가져올 값이 없는 상태를 의미
* 모든 참조유형에 대한 기본 값
* 유효한 객체 인스턴스가 아니므로 할당되는 메모리가 없음
### NullPointerException 예외가 발생하는 경우
* null 객체에서 메서드를 호출하는 경우
* null 객체의 필드에 접근하거나 값을 변경하는 경우
* null의 길이를 배열처럼 다루려는 경우
* null을 통해 동기화를 할 경우
* null을 throw 하는 경우
### Null 값이 필요한 경우
* 참조 변수의 값이 할당되지 않았음을 나타내는 데 사용
* LinkedList 및 Tree 같은 자료 구조를 구현하는 데 사용
* 싱글턴 패턴 등에 사용
### NullPointerException 을 피하는 경우들
1. 문자열 비교
    * String 변수와 리터럴 문자를 비교할 때
```java
String str = null;
// NullPointerException 발생
if(str.equals("hello"))
	...
```
**해결법**
```java
String str = null;
// 리터럴문자에서 equals 메서드를 호출함으로써 해결
if("hello".equals(str))
	...
```
2. 삼항연산자 사용
```java
String str = null;
// NullPointerException 발생
System.out.println(str.length());
```
**해결법**
```java
String str = null;
// 삼항연산자를 사용하여 null일 경우 처리함으로써 해결
System.out.println((str == null) ? "null" : str.length());
```
3. `Optional` 사용
* Java 8 버전부터 추가된 `Optional` 클래스를 사용하는 방법
```java
Optional<Member> maybeNotMember = Optional.ofNullable(searchMember)
        .orElse(null);
```
* [Optional 자세한 설명](./Optional.md)
