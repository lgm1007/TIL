# @Async
* Spring 프레임워크에서 제공해주는 메서드를 비동기적으로 실행할 수 있도록 지원하는 어노테이션
* 비동기적으로 메서드를 호출하여 원하는 작업을 병렬로 처리할 수 있다.
### 작동 원리
* `@Async` 어노테이션을 사용하면 메서드의 호출이 별도의 스레드에서 비동기적으로 실행된다.
* Spring은 내부적으로 `TaskExecutor`라는 빈을 사용하여 비동기 작업을 관리한다.
### 설정
1. Spring Boot 프로젝트에 `@EnableAsync` 어노테이션을 추가하여 비동기 기능을 활성화한다.
2. 비동기 메서드를 호출하고자 하는 클래스에 `@Service` 등과 함께 `@Async` 어노테이션을 사용한다.
3. 메서드 반환 타입은 `void` 또는 `Future<T>` 형태여야 한다.
### 사용 방법
1. `@EnableAsync` 어노테이션을 설정 클래스에 추가한다.
```java
@Configuration
@EnableAsync
public class AsyncConfig {
	// 설정 내용 (ThreadPool, 기타 설정 등)
}
```
2. `@Async` 어노테이션을 사용하여 메서드를 호출한다.
```java
@Service
public class MyService {
	@Async
    public void asyncMethod() {
		// 비동기적으로 실행될 내용
    }
}
```
### 사용 예제
```java
@Service
public class MyService {
	@Async
    public CompletableFuture<String> asyncMethod() {
		return CompletableFuture.completedFuture("Async method completed.");
    }
}
```
### 주의사항
1. `@Async` 어노테이션이 선언된 메서드는 반드시 `public` 으로 선언해야 한다.
2. 비동기 메서드는 같은 클래스 내에서 호출할 수 없다. 메서드 호출은 스프링 프록시를 통해 이루어지므로 비동기 메서드는 동일한 클래스 내에서 호출되어도 비동기로 실행되지 않는다.
3. 메서드의 매개변수는 자동으로 복사되어 비동기 스레드에서 사용된다. 하지만 레퍼런스나 컨텍스트에 대한 처리가 필요한 경우 별도의 조치가 필요하다.
