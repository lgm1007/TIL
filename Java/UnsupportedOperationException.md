# UnsupportedOperationException
* 일반적으로 **List 타입을 new 연산자로 초기화하지 않은 상태에서 List의 값을 변경하려 할 때** 발생하는 에러
### 예외 예제
```java
public static void main(String[]args){
    List<String> list = Arrays.asList("apple");
    System.out.println(list);

    list.add("banana");
    System.out.println(list);
}
```
* List 타입을 `Arrays.asList()` 로 초기화했을 때는 문제가 없으나, list의 값을 변경하려 할 때 에러가 발생한다.
### 해결 방법
```java
public static void main(String[]args){
    List<String> list = new ArrayList<>(Arrays.asList("apple"));
    System.out.println(list);

    list.add("banana");
    System.out.println(list);
}
```
