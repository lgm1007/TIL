# @Component와 @Bean
* 알고 사용하자.
### @Component
* 싱글톤 클래스 빈을 생성하는 어노테이션
  * `@Scope("Prototype")` 어노테이션을 통해 싱글톤이 아닌 빈을 생성할 수도 있다.
* `@Service`, `@Repository` 어노테이션 또한 `@Component` 에 포함
* 선언적인 어노테이션으로, 스프링의 **컴포넌트 스캔**을 통해 정의한 클래스를 **빈으로 등록**한다.
  * 해당 클래스의 인스턴스를 생성하고 의존성 주입을 수행할 수 있다.
### @Bean
* 메서드가 반환하는 객체를 스프링의 **IoC 컨테이너에 등록**하는 어노테이션
  * `@Bean` 어노테이션이 붙은 메서드를 호출하여 반환된 객체는 스프링 컨테이너에 등록되며, 이후 해당 객체를 다른 클래스에서 `@Autowired` 어노테이션으로 주입받아 사용할 수 있다.
* 주로 `@Configuration` 어노테이션이 들어간 Spring을 설정하는 클래스 내에 들어가는 메서드에서 선언한다.
  * `@Configuration` 클래스는 **스프링 IoC 컨테이너에 등록되어야 하는 빈들**을 정의하는 클래스이다.
* 예제 소스
```java
@Configuration
public class AppConfig {
    
    @Bean
    public DataSource dataSource() {
        // DataSource를 생성하고 반환
    }
    
    @Bean
    public JdbcTemplace jdbcTemplace(DataSource dataSource) {
        return new JdbcTemplace(dataSource);
    }
    
}
```
* DataSource와 JdbcTemplace을 Bean으로 등록
* `jdbcTemplace()` 메서드에서 jdbcTemplace이 의존하고 있는 DataSource 객체를 생성자를 통해 주입받아 JdbcTemplate 객체를 생성한다.
* 이후 이러한 빈들은 다른 클래스에서 `@Autowired` 로 주입받아 사용 가능하다.

