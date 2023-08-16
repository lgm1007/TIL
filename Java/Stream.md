# Stream
### Stream 이란?
* Java 8부터 지원하는 기능
* 컬렉션, 배열 등에 대해 저장되어 있는 요소들을 하나씩 참조하여 반복적인 처리를 가능하게 하는 기능
* 불필요한 for문과 그 안에서 동작하는 if문 등의 분기 처리를 하지 않고 깔끔하고 직관적인 코드로 구현할 수 있음
### Stream 특징
1. **데이터를 변경하지 않음**
    * Stream은 원본데이터로부터 데이터를 읽기만 할 뿐, 원본데이터 자체를 변경하지 않음
2. **일회용이다.**
    * Stream은 한 번 사용하면 닫혀서 재사용이 불가능함
    * 필요하다면 정렬된 결과를 컬렉션이나 배열에 담아 반환할 수 있음
3. 작업을 **내부 반복으로 처리**함
    * 내부 반복은 반복문을 메서드의 내부에 숨길 수 있다는 것을 의미
    * 즉, 반복문이 코드 상에 노출되지 않음 
### Stream 구조
* Stream의 세 가지 구조
    1. Stream 생성
    2. 중개 연산
    3. 최종 연산
* **데이터 소스 객체 집합.Stream생성().중개연산().최종연산()**;
* 예시 코드
```java
String[] strArray = {"red", "yellow", "blue", "green", "brown"};
Set<String> colorSet = Arrays.asList(strArray)              // strArray를 List로 변환
			.stream()                           // 1. Stream 생성
			.filter(x -> x.contains("b"))       // 2. 중개 연산 : "b"가 포함된 단어만 
			.collect(Collectors.toSet());       // 3. 최종 연산 : 중개 연산을 통해 가공된 stream을 Set 형태로 모아줌
colorSet.forEach(x -> System.out.println(x));               // 출력: blue brown
```
### Stream 사용 가능 데이터소스
1. 컬렉션
2. 배열
3. 가변 매개변수
4. 지정된 범위의 연속된 정수
5. 특정 타입의 난수들
6. 람다 표현식
7. 파일
8. 빈 스트림
### 중개 연산
* 대표적인 중개 연산과 그에 따른 메서드
    1. Stream 필터링: **filter()**, **distinct()**
    2. Stream 변환: **map()**, **flatMap()**
    3. Stream 제한: **limit()**, **skip()**
    4. Stream 정렬: **sorted()**
    5. Stream 연산 결과 확인: **peek()**
### 최종 연산
* 최종 연산은 앞서 중개 연산을 통해 만들어진 stream에 있는 요소들에 대해 마지막으로 각 요소를 소모하며 최종 결과를 표시
* 지연(lazy)되었던 모든 중개 연산들이 최종 연산시에 모두 수행됨
* 최종 연산 시에 **모든 요소를 소모한 해당 stream은 더 이상 사용할 수 없음**
* 대표적인 최종 연산
    1. 요소의 출력: **forEach()**
    2. 요소의 소모: **reduce()**
    3. 요소의 검색: **findFirst()**, **findAny()**
    4. 요소의 검사: **anyMatch()**, **allMatch()**, **noneMatch()**
    5. 요소의 통계: **count()**, **min()**, **max()**
    6. 요소의 연산: **sum()**, **average()**
    7. 요소의 수집: **collect()**
#### allMatch() 사용 시 주의할 점
* `allMatch()`는 컬렉션 내 모든 요소들이 주어진 조건에 만족하는지 여부를 검사하는 메서드이다.
* 하지만 만약 컬렉션이 **비어있는 경우**라면 어떨까?
```java
@Test
public void allMatchTestWhenEmpty() {
	List<Member> members = new AraryList<>();
	
	boolean result = members.stream()
        .allMatch(member -> member.getAge() > 20);
	
	assertThat(result).isFalse();
}
```
* `members` 회원 리스트가 비어있고 20살 이상의 회원이 한 명도 없기 때문에 조건에 맞지 않아 `false`를 반환할 것으로 보통 예상할 것이다.
* 하지만 `allMatch()` 메서드는 `true`를 반환한다.
* 이는 **Vacuous Truth** 라는 논리학 개념에서 비롯된 것으로, `P이면 Q이다.` 라는 명제에서 P가 거짓이면 Q는 참이던 거짓이던 상관없이 전체 명제는 참이 된다는 개념이다.

