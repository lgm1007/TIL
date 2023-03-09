# slf4j
* 자바 애플리케이션에서 로깅 기능을 사용하기 위한 인터페이스
* 추상 로깅 프레임워크로 단독으로 사용하지는 않으며, 사용자가 배포 시 원하는 로깅 프레임워크를 결정하고 사용할 수 있다.
* 코드 변경 없이 로깅 구현체를 변경할 수 있는 유연성 제공
### 장점
1. **로깅 구현체와의 분리**
   * 로깅 기능 호출 시 로깅 구현체를 직접 지정하지 않는다.
   * slf4j가 로깅 구현체를 찾아 로깅 기능 처리
   * 코드 변경 없이 로깅 구현체 변경 가능
2. **성능**
   * 로깅 구현체가 로깅 기능을 사용하지 않는 경우 메시지 생성이 발생하지 않음
     * 불필요한 메시지 생성으로 인한 성능 저하 방지
3. **다양한 로깅 구현체 지원**
   * Log4j, java.util.logging, Logback 등 다양한 로깅 구현체 지원
### 단점
1. 다른 로깅 프레임워크에 비해 제한적인 기능
   * 예로, Logback이나 Log4j2는 slf4j 처럼 로깅 인터페이스를 제공하지만 slf4j보다 더 많은 로깅 기능을 제공한다.
2. 로깅 구현체에 따른 호환성 문제 발생 가능
   * 로깅 구현체를 변경할 때 호환성 문제가 발생할 수 있다.
   * slf4j에서 Log4j2를 사용하고 있다가 Logback으로 변경하려고 할 때, Log4j2의 기능을 사용하는 코드가 있을 경우 호환성 문제 발생
### 예제
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class SampleClass {
	private static final Logger logger = LoggerFactory.getLogger(SampleClass.class);
	
	public void loggingTest() {
		logger.debug("SampleClass.loggingTest() is called.");
		// do Something...
		logger.info("loggingTest done.");
	}
}
```
* 위 예제 소스는 slf4j 심플 로깅 구현체로 구현한 소스
* 위 로그는 일반적으로 다음과 같이 출력됨 (정상적으로 메서드 기능이 동작했다는 가정 하에)
```csharp
[main] DEBUG SampleClass - SampleClass.loggingTest() is called.
[main] INFO SampleClass - loggingTest done.
```

