# 스프링 부트 (Spring Boot)
### 스프링 부트 이해
* Spring 프레임워크 기반으로 복잡한 설정 없이 쉽고 빠르게 프로젝트를 구성해주는 라이브러리
* Spring 프레임워크를 사용하기 위한 필수 설정파일 (web.xml, rootContext.xml, ServletContext.xml 등...) 을 일일이 작성하고 설정하는 복잡한 과정을 쉽고 빠르게 해결해줌
### 스프링 부트 특징
1. **라이브러리 관리 자동화**
    * 스프링부트의 Starter 라이브러리를 등록하여 라이브러리 의존성을 간단히 관리할 수 있다.
2. **라이브러리 버전 자동관리**
    * pom.xml 내 스프링 부트 버전을 작성하여 스프링 라이브러리 뿐 아니라 서드 파티 (third party) 라이브러리들에 호환되는 버전으로 자동으로 다운로드 및 관리해준다.
3. **설정 자동화**
    * `@EnableAutoConfiguration` 어노테이션을 선언해 스프링에서 자주 사용했던 설정들을 알아서 등록한다.
    * 요즘에는 `@EnableAutoConfiguration + @SpringBootConfiguration + @ComponentScan` 으로 구성된 `@SpringBootApplication` 어노테이션을 사용한다.
4. **내장 Tomcat**
    * `@SpringBootApplication` 어노테이션이 선언되어 있는 클래스의 `main()` 메서드를 실행하는 것만으로 서버를 구동시킬 수 있다.
5. **독립적으로 실행 가능한 jar**
    * war 파일로 패키징해야 했던 웹 프로젝트와 달리, 내장 Tomcat을 지원하기 때문에 jar 파일로 패키징하여 웹 애플리케이션 실행이 가능하다.
