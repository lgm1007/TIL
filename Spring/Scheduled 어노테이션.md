# @Scheduled
### Spring Scheduler
* 일정한 시간 간격으로 또는 특정 일정에 메서드를 실행되도록 하는 기능
* Spring Boot starter 에 기본적으로 의존 `org.springframework.scheduling`
### @Scheduled 규칙
* 메서드는 void 타입이어야 한다.
* 메서드는 매개변수 사용 불가
### @Scheduled 옵션
1. **fixedDelay**
	* 이전 작업이 종료된 후 설정시간 (milliseconds) 이후 다시 시작
    * 이전 작업이 완료될 때까지 대기
2. **fixedRate**
	* 고정 시간 간격으로 시작
    * 이전 작업이 완료될 때까지 다음 작업이 진행되지 않음
    * 병렬 동작을 사용할 경우 `@Async` 추가
      * 클래스에 `@EnableAsync` 추가
   * 예제 코드
```java
@Async
@Scheduled(fixedRate = 1000)
public void scheduleFixedRate() throws InterruptedException {
	log.info("Fixed rate scheduled - {}", System.currentTimeMillis() / 1000);
	Thread.sleep(1000);
}
```
3. **fixedDelay + initialDelay**
	* `@initialDelay` 값 이후 처음 실행되고, `@fixedDelay` 값에 따라 계속 수행
	* 예제 코드
```java
@Scheduled(fixedDelay = 1000, initialDelay = 3000)
public void scheduleFixedDelayAndInitialDelayTask() {
	log.info("FixedDelay and InitialDelay scheduled - {}", System.currentTimeMillis() / 1000);
}
```
4. **Cron**
	* 직접 예약으로 수행
	* 예제 코드
```java
@Scheduled(cron = "0 15 11 20 * ?") // 매월 15일 오전 11시 20분에 실행
public void scheduledCronTask() {
	log.info("Cron scheduled - {}", System.currentTimeMillis() / 1000);
}
```
