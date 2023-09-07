# Future와 ComplatableFuture
### Future 인터페이스
* `Future` 인터페이스는 비동기 작업의 결과를 나타내는 인터페이스로, Java 5 부터 도입되었다.
* `Future`는 비동기 작업의 결과를 나타내는데 사용되며, 작업이 완료될 때까지 기다리지 않고 다른 작업을 수행할 수 있게 해준다.
### Future의 주요 메서드
1. `boolean cancel(boolean mayInterruptIfRunning)`: 현재 실행 중인 작업을 취소한다. `mayInterruptIfRunning` 파라미터는 작업이 이미 시작되었을 때 작업을 중단할지 여부를 지정한다.
2. `boolean isCancelled()`: 작업이 취소되었는지 여부를 반환한다.
3. `boolean isDone()`: 작업이 완료되었는지 여부를 반환한다.
4. `V get() throws InterruptedException, ExecutionException`: 작업의 결과를 가져온다. 작업이 완료될 때까지 블록된다. 만약 작업이 이미 취소되었다면 `CancellationException`이 발생하고, 작업 실행 중 예외가 발생하면 `ExecutionException`이 발생한다.
5. `V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException`: 일정 시간 동안 작업의 결과를 가져온다. 지정된 시간 안에 작업이 완료되지 않으면 `TimeoutException`이 발생한다.
### Future의 비동기적 프로그래밍 처리 방법
`Future`를 사용하여 비동기적 작업을 처리하는 일반적인 방법은 아래와 같다.

1. **작업 생성**: `ExecutorService` 또는 `CompletableFuture` 등을 사용하여 비동기 작업을 생성한다.
2. **`Future` 얻기**: 비동기 작업을 제출하면 `Future` 객체를 얻는다. 이 `Future` 객체를 통해 작업의 상태를 추적하고 결과를 얻을 수 있다.
3. **비동기 작업 실행**: 비동기 작업은 백그라운드 스레드 또는 다른 실행 컨텍스트에서 실행된다.
4. **결과 얻기**: 작업이 완료되면 `get()` 메서드를 사용하여 작업의 결과를 가져온다. 작업이 완료되지 않았다면 `get()` 메서드는 블록된다.
5. **예외 처리**: 작업 실행 중 예외가 발생하면 `ExecutionException`을 통해 예외를 처리한다.
6. **작업 취소**: 작업이 불필요하게 길어질 경우 `cancel()` 메서드를 사용하여 작업을 취소할 수 있다.
### Future의 사용 예제
```java
ExecutorService executor = Executors.newFixedThreadPool(2);
Future<Integer> future = executor.submit(() -> {
    // 비동기 작업 수행
    return 42;
});

// 다른 작업 수행

try {
    Integer result = future.get(); // 작업 결과 가져오기 (블록됨)
    System.out.println("결과: " + result);
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}

executor.shutdown();
```
* Java 8 부터는 `CompletableFuture`와 같은 더 강력하고 편리한 비동기 프로그래밍 기능이 추가되었다.
### CompletableFuture 클래스
* Future 인터페이스에서는 다음과 같은 단점이 존재한다.
1. 외부에서 완료시킬 수 없고 `get()` 메서드의 타임아웃 설정으로만 완료 가능
2. `get()`을 통해서만 이후의 결과를 처리할 수 있다.
3. 여러 `Future`를 조합할 수 없다.
* 기존 `Future` 인터페이스를 기반으로 외부에서 완료시킬 수 있는 클래스인 `CompletableFuture`가 탄생했다.
### CompletableFuture의 장점
* `Future`와 달리 외부에서 명시적으로 Complete를 시킬 수 있다.
* 명시적으로 `Executor`를 사용할 필요가 없다.
  * `CompletableFuture` 만으로 비동기적 작업들을 실행할 수 있다.
### CompletableFuture의 비동기적 프로그래밍 처리 방법
리턴값이 없는 경우: `runAsync()`
* 형태 예제
```java
// runnable: 비동기로 실행할 작업을 나타내는 Runnable 객체
public static CompletableFuture<Void> runAsync(Runnable runnable)
```
* 예제 소스
```java
public class RunAsyncExample {
    public static void main(String[] args) {
        // 비동기 작업 시작
        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("비동기 작업이 완료되었습니다.");
        });

        // 비동기 작업이 완료될 때까지 대기
        future.join();

        System.out.println("메인 스레드 계속 실행됨.");
    }
}
```
<br/>

리턴값이 있는 경우: `supplyAsync()`
* 형태 예제
```java
// supplier: 비동기로 실행할 작업을 나타내는 Supplier 객체로, 결과를 반환한다.
public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier)
```
* 예제 소스
```java
public class SupplyAsyncExample {
    public static void main(String[] args) {
        // 비동기 작업 시작
        CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return 42;
        });

        // 비동기 작업이 완료되면 결과를 출력
        future.thenAccept(result -> {
            System.out.println("비동기 작업 결과: " + result);
        });

        System.out.println("메인 스레드 계속 실행됨.");
    }
}
```

* 위 `CompletableFuture` 메서드로 비동기 작업을 간단하게 처리할 수 있고, 결과를 기다리지 않고 다른 작업을 수행할 수도 있으며, 작업이 완료되면 필요한 후속 작업을 실행할 수도 있다.