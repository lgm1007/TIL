# AOP
### AOP 개념
* AOP(Aspect-Oriented Programming) 은 애플리케이션에서 일부 공통적인 기능을 분리하여 재사용하기 위한 프로그래밍 패러다임이다.
* 어떤 로직을 기준으로 **핵심적인 관점, 부가적인 관점**으로 나누어서 보고 해당 관점을 기준으로 모듈화한다.
  * 흩어진 관심사를 모듈화할 수 있는 프로그래밍 기법  
  
<img width="362" alt="aop-example" src="https://user-images.githubusercontent.com/80727821/230023126-ebb606da-ed16-4790-8199-088e280cd9bd.png"><br/>
* 위 그림에서 클래스 A, B, C에 공통으로 나타나는 색상의 블록은 중복되는 메서드, 필드 등이다.
* 예로 클래스 A의 노란색 블록 코드를 수정해야 한다면, 다른 클래스들에서 해당 블록을 찾아서 일일이 수정해야 하는 상황이 발생한다.
  * SOLID 원칙을 위배함
* 그림의 아래처럼 같은 색상의 블록을 모듈화 시켜놓고 어디에 적용시킬지만 정의해주면 된다.
### AOP 구성
* AOP는 주로 메서드 호출 전/후, 예외 발생 시 등 공통적인 로직(Aspect)을 분리하여 관리한다.
* 이 때 Aspect는 아래와 같이 구성된다.
1. **Advice**
    * 실제 수행할 공통 로직을 정의한다.
    * Before, After, Around 등이 존재한다.
2. **Join point**
    * Advice가 실행될 수 있는 지점
    * 메서드 호출 전/후, 예외 발생 시 등이 Join point 이다.
3. **Pointcut**
    * Join point의 집합으로, Advice가 적용될 Join point를 선별한다.
4. **Weaving**
    * Aspect를 적용하는 과정이다.
    * Aspect가 적용되어 동작할 수 있도록 코드에 삽입하는 것을 의미한다.
### AOP 주 사용처
1. 로깅
2. 트랜잭션 관리
3. 보안
### AOP 적용 방법
1. **어노테이션 기반** 설정
    * `@Aspect` 어노테이션을 사용하여 Aspect 클래스를 정의하고, `@Pointcut` 어노테이션을 사용하여 Pointcut을 정의한다.
    * Advice 메서드에는 `@Before`, `@After`, `@Around` 등의 Advice 어노테이션을 사용하여 메서드 실행 전/후, 예외 발생 시 등에 실행될 코드를 작성한다.
    * 이후 `@Pointcut` 어노테이션을 사용하여 Advice가 실행될 Join point를 지정한다.
    * ```java
      // LoggingAspect.java
      @Aspect
      @Component
      public class LoggingAspect {
        @Before("execution(* com.example.service.*.*(..))")
        public void beforeAdvice() {
            System.out.println("메서드 실행 전 로깅 수행");
        }
      }
      
      // UserService.java
      @Service
      public class UserService {
        public void createUser(String name) {
            System.out.println("사용자 생성: " + name);
        }
      }
      
      // MainApp.java
      public class MainApp {
        public static void main(String[] args) {
            ApplicationContext context = new ApplicationConfigApplicationContext(AppConfig.class);
            UserService userService = context.getBean(UserService.class);
            userService.createUser("John");
        }
      }
      
      // AppConfig.java
      @Configuration
      @EnableAspectJAutoProxy
      @ComponentScan(basePackages = "com.example")
      public class AppConfig {
      }
      ```
2. XML 기반 설정
    * XML 설정 파일에 `<aop:config>` 요소를 사용하여 Aspect와 Pointcut, Advice를 정의한다.
    * `<aop:config>` 요소 내에서 `<aop:aspect>` 요소를 사용하여 Aspect를 정의하고, `<aop:pointcut>`과 `<aop:advice>` 요소를 사용하여 Pointcut과 Advice를 정의한다.
    * ```xml
      <!-- applicationContext.xml -->
      <beans xmlns:aop="http://www.springframework.org/schema/aop"
      xsi:schemaLocation="http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

        <!-- Aspect 정의 -->
        <aop:config>
           <aop:aspect id="loggingAspect" ref="loggingAspectBean">
               <!-- Pointcut 정의 -->
               <aop:pointcut id="executionPointcut" expression="execution(* com.example.service.*.*(..))" />
               <!-- Advice 정의 -->
               <aop:before pointcut-ref="executionPointcut" method="beforeAdvice" />
           </aop:aspect>
        </aop:config>
   
        <!-- Aspect Bean 정의 -->
        <bean id="loggingAspectBean" class="com.example.aspect.LoggingAspect" />
   
        <!-- 서비스 Bean 정의 -->
        <bean id="userService" class="com.example.service.UserService" />
      </beans>
      ```
   * ```java
     // LoggingAspect.java
     public class LoggingAspect {
        public void beforeAdvice() {
            System.out.println("메서드 실행 전 로깅 수행");
        }
     }
     
     // UserService.java
     public class UserService {
        public void createUser(String name) {
            System.out.println("사용자 생성: " + name);
        }
     }
     
     // MainApp.java
     public class MainApp {
        public static void main(String[] args) {
            ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
            UserService userService = (UserService) context.getBean("userService");
            userService.createUser("John");
        }
     }
     ```


