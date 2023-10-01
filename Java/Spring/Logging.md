# Logging
### Logging 설명
* 정보를 제공하는 일련의 기록인 로그(log)를 생성하는 시스템을 작성하는 행동
* 버그에 대한 유용한 정보를 제공할 수 있다.
* 성능에 대한 통계와 같은 정보를 제공할 수 있다.
### Logging 라이브러리 종류
1. java.util.logging
    * jdk 1.4 부터 포함된 표준 로깅 라이브러리
    * 별도 라이브러리 추가가 불필요하다.
    * 기능이 많이 부족해 주로 다른 라이브러리를 사용한다.
2. Apache Commons logging
    * 아파치 재단의 Commons 라이브러리 중에 로그 출력을 제공하는 라이브러리
3. log4j2
    * 아파치 재단에서 제공하는 로깅 라이브러리
    * 21년 12월 즘 **보안 취약점이 보고**되었다.
      * [관련 링크](https://cloud.google.com/log4j2-security-advisory?hl=ko)
4. logback
    * log4j를 개발한 Ceki Gulcu가 log4j의 단점을 개선하여 개발된 로깅 프레임워크
    * 주로 Spring Boot 에서 기본적으로 logback 프레임워크를 사용한다.
5. slf4j
    * 퍼싸드(Facade) 디자인 패턴에 따라 만들어진 클래스
    * 퍼싸드 디자인 패턴 덕분에 다양한 구현체를 하나의 통일된 방식으로 사용 가능하다.
    * 로깅에 대한 추상 레이어를 제공하는 인터페이스 모음
### Logging 적용 예제
1. logback 적용
   * 의존성 추가
     * Maven 사용
       * ```xml
         <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.6</version> <!--버전은 필요에 따라 업데이트-->
         </dependency>
         ```
     * Gradle 사용
       * ```groovy
         implementation 'ch.qos.logback:logback-classic:1.2.6' // 로그백 버전은 필요에 따라 업데이트할 수 있습니다.
         ```
   * 로그 설정 파일 생성
     * 로그백 구성을 위해 `logback.xml` 파일을 생성하고 클래스패스 루트에 위치시킨다.
       * ```xml
         <configuration>
            <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
                <encoder>
                    <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
                </encoder>
            </appender>
            <root level="INFO">
                <appender-ref ref="console"/>
            </root>
         </configuration>
         ```
   * Spring Boot 애플리케이션에서 로깅 사용
     * Java 클래스에서 `org.slf4j.Logger` 인터페이스를 사용하여 로그를 기록한다.
       * ```java
         import org.slf4j.Logger;
         import org.slf4j.LoggerFactory;
         import org.springframework.stereotype.Service;

         @Service
         public class MyService {
             private static final Logger logger = LoggerFactory.getLogger(MyService.class);

             public void doSomething() {
                 logger.debug("Debug 로그");
                 logger.info("Info 로그");
                 logger.warn("경고 로그");
                 logger.error("에러 로그");
             }
         }
         ```
### Logging을 하는 이유
* 스레드 정보, 클래스 이름과 같은 부가 정보를 함께 확인할 수 있고, 출력 모양을 조정할 수 있다.
* 로그 레벨에 따라 개발 서버는 모든 로그, 운영 서버는 에러 로그만 출력하는 등 상황에 맞게 조절할 수 있다.
* `System.out` 콘솔에만 출력하는 것이 아니라 파일이나 네트워크 등 로그를 별도의 위치에 남길 수 있다.
