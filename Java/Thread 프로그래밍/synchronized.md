# synchronized
### synchronized 키워드
* 멀티스레드 환경에서 동일한 자원에 대한 동시 접근을 막는 방식
* synchronized 키워드를 붙인 자원은 동시에 접근할 수 없다.
* 여러 스레드에서 해당 자원에 동시에 접근할 경우, 가장 처음 접근한 스레드가 작업을 끝낼 때까지 **자원에 lock을 걸어서 다른 스레드에서의 접근을 완전 차단**한다.

### 사용 방법
* synchronized 키워드를 사용하는 방법은 `synchronized 메서드`와 `synchronized 블록` 두 가지 방식이 있다.
* **synchronized 메서드**
  * 해당 메서드에 오직 하나의 스레드만 접근 가능하게 하는 방법
```java
public synchronized void doSomething() {}
```

* **synchronized 블록**
  * 해당 블록 내에 있는 구문은 하나의 스레드에서만 접근 가능하다.
  * 동기화가 필요한 로직에만 lock을 걸도록 하여 메서드 접근 자체에 lock을 거는 synchronized 메서드보다 성능 저하를 줄일 수 있다.
```java
synchronized(object) {
    // do something ...
}
```

### 문제점
* synchronized를 남용할 경우 lock이 걸리는 스레드가 많아지고, synchronized 메서드 또는 로직에 대해 병목현상이 발생하기 쉬워진다.
* 따라서 synchronized 키워드 대신 Atomic 변수나 volatile 키워드를 사용하는 추세이다.
