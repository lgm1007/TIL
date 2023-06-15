# Spring - Retry
### spring-retry 라이브러리
* 메서드 실행 중 일시적인 오류 또는 예외 상황에서 자동으로 메서드를 재시도할 수 있도록 도와주는 라이브러리
### 의존성
* in Maven
```
<!-- https://mvnrepository.com/artifact/org.springframework.retry/spring-retry -->
<dependency>
    <groupId>org.springframework.retry</groupId>
    <artifactId>spring-retry</artifactId>
    <version>{사용하려는 버전}</version>
</dependency>
```
* in Gradle
```
// https://mvnrepository.com/artifact/org.springframework.retry/spring-retry
implementation 'org.springframework.retry:spring-retry:{사용하려는 버전}}'
```
### 주요 기능
1. `@Retryable` 어노테이션
    * **재시도 로직을 적용할 메서드**에 사용하는 어노테이션
    * 어노테이션의 속성을 설정하여 동작 제어
```java
@Service
public class MyService {
    
    @Retryable(
            value = { NetworkException.class },
            maxAttempts = 3,
            backoff = @Backoff(delay = 2000)
    )
    public void fetch() {
        // NetworkException이 발생할 경우, 최대 3번 재시도하고 2초 간격으로 딜레이
    }
}
```
2. 백오프 (Backoff) 전략
    * **재시도 시간 간격**을 조절하는 전략
    * `@Retryable` 어노테이션의 `backoff` 속성을 설정하여 초기 딜레이, 최대 재시간 시간 등을 구성할 수 있다.
```java
// ...

    @Retryable(
            value = { NetworkException.class },
            maxAttempts = 5,
            backoff = @Backoff(delay = 100, multiplier = 2)
    )
    public void fetch() {
        // NetworkException 이 발생할 경우, 최대 5번 재시도하고 지수적으로 증가하는 딜레이 적용
    }
```
3. 재시도 정책 (Retry Policy)
    * 재시도 **동작**을 제어하는 정책
    * 예외 타입, 최대 재시간 횟수 등을 설정하여 언제 재시도할지를 결정할 수 있다.
4. RetryTemplate
    * `@Retryable` 어노테이션 외에도 **프로그래밍 방식**으로 재시도 로직을 구현할 수 있도록 도와주는 유틸리티 클래스
```java
@Service
public class MyService {
    
    private final RetryTemplate retryTemplate;
    
    public MyService() {
        retryTemplate = new RetrtyTemplate();
        FixedBackoffPolicy backoffPolicy = new FixedBackoffPolicy();

        backoffPolicy.setBackoffPeriod(500);
        retryTemplate.setBackoffPolicy(backoffPolicy);
        
        SimpleRetryPolicy simpleRetryPolicy = new SimpleRetryPolicy();

        simpleRetryPolicy.setMaxAttempts(3);
        retryTemplate.setRetryPolicy(simpleRetryPolicy);
    }
    
    public void fetch() {
        retryTemplate.execute(context -> {
            // 외부 서비스 호출 또는 데이터 로드 작업 수행
            return null;    // 결과 반환
        });
    }
}
```
* `RetryTemplate`를 사용하여 재시도 로직을 프로그래밍 방식으로 구현한 예제 소스
  * `backoffPolicy`와 `RetryPolicy`를 설정하여 재시도 간격과 최대 재시도 횟수를 지정하고, `execute()` 메서드 내에서 재시도할 작업을 수행한다.
 
