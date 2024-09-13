# Spring Bean 등록 방법 및 @Configuration에서 @Bean을 사용해야 하는 이유
### 들어가기 앞서
* `@Bean`에 대한 자세한 설명은 다음 문서에서 확인 ([@Component와 @Bean](@Component와%20@Bean.md))

### Spring Bean 등록 방법
#### 1. `@Configuration` 어노테이션과 `@Bean` 어노테이션 사용하기
* 예를 들어 다음과 같은 클래스가 있고 해당 클래스를 스프링 컨테이너에 등록하고자 한다고 가정하자.
```java
public class BeanResource {}
```

* 클래스를 빈으로 등록해주기 위해 명시적으로 설정 클래스에서 `@Bean` 어노테이션을 사용해 **수동으로** 스프링 컨테이너에 빈을 등록하는 방법이 있다.
* `@Bean`을 사용해 빈을 등록해줄 때는 **메서드 이름이 빈 이름으로 결정**되기 때문에 중복된 빈 이름이 존재하지 않는지 주의해야 한다.
```java
@Configuration
public class BeanResourceConfig {
    @Bean
    public BeanResource beanResource() {
        return new BeanResource();
    }
}
```

* `@Configuration` 어노테이션이 붙은 설정 클래스에서 `@Bean`이 빈으로 등록되는 과정은 다음과 같다.
  * 스프링 컨테이너 `@Configuration`이 붙은 클래스를 자동으로 빈으로 등록해두고, 해당 클래스를 파싱해서 `@Bean`이 붙은 메서드를 찾아 빈을 생성해준다.
  * 스프링 빈으로 등록된 다른 클래스 내에서도 `@Bean`으로 빈을 등록해주는 것도 가능은 하다. 하지만 `@Configuration` 안에서 `@Bean`을 사용해야 싱글톤을 보장받을 수 있다.
  * `@Configuration`과 `@Bean` 사용에 대한 자세한 내용은 아래에서 다루도록 하겠음

* 개발자가 수동으로 빈을 등록해야 하는 상황은 주로 다음과 같은 상황이다.
  1. 개발자가 직접 제어가 불가능한 라이브러리를 활용할 때
  2. 어플리케이션 전 범위적으로 사용되는 클래스를 등록할 때
  3. 다형성을 활용하여 여러 구현체를 등록해주어야 할 때

* 여러 구현체를 빈으로 등록한 후 어떤 구현체들이 빈으로 등록되었는지 파악하기 위해 `@Configuration` 클래스만 확인하면 된다.

#### 2. `@Component` 어노테이션 사용하기
* 수동으로 직접 빈을 등록하는 작업은 빈으로 등록할 클래스가 많아질수록 많은 시간이 소요될 것이며 번거로운 작업이 될 것이다.
* 스프링에는 `@Component` 어노테이션이 붙은 클래스를 자동으로 찾아서 빈으로 등록해주는 **컴포넌트 스캔** 기능을 제공한다.
* 직접 개발한 클래스를 편하게 빈으로 등록하고자 할 때는 `@Component` 어노테이션을 활용한다.
* `@Component` 어노테이션을 갖는 어노테이션으로는 `@Service`, `@Controller`, `@Repository` 등이 있으며 `@Configuration` 또한 `@Component` 어노테이션을 갖고 있다.

* 직접 개발한 클래스는 `@Component` 어노테이션을 통해 빈으로 등록하는 것이 좋다.
  * 해당 클래스에 있는 `@Component` 어노테이션을 보고 해당 클래스가 빈으로 등록되어 있는지 바로 확인할 수 있기 때문이다.
  * `@Component`를 사용한다면 Main 또는 Application 클래스에서 `@ComponentScan`으로 컴포넌트를 스캔하는 범위를 지정해주어야 한다.
  * 하지만 Spring Boot를 사용한다면 `@SpringBootConfiguration` 하위에 기본적으로 포함되어 있어 별도 설정을 해주지 않아도 무방하다.

### `@Bean`을 반드시 `@Configuration` 안에서 사용해야 하는 이유
#### `@Configuration`에 적용되는 프록시 패턴
* 위에서 설명한 것처럼 `@Configuration` 어노테이션 안에는 `@Component` 어노테이션을 포함하고 있어서 `@Configuration`이 붙은 클래스 역시 빈으로 등록된다.
* 그럼에도 불구하고 `@Configuration`가 따로 존재하는 이유는 CGLib으로 프록시 패턴을 적용해 수동으로 등록하는 빈이 **반드시 싱글톤으로 생성됨을 보장하기 위해서**이다.
* 예를 들어 `BeanResource` 클래스를 `@Component` 어노테이션을 사용해 자동으로 빈 등록을 한다면, 스프링이 해당 클래스의 객체 생성을 제어하여 1개의 객체만 생성되도록 한다.
* 하지만 `@Bean`으로 수동으로 직접 빈 등록을 해준다고 하면, 아래와 같이 빈 등록 메서드를 여러 번 호출할 수도 있게 된다.
```java
@Configuration
public class BeanResourceConfig {
    @Bean
    public BeanResource beanResource() {
        return new BeanResource();
    }
    
    @Bean
    public SecondBeanResource secondBeanResource() {
        return new SecondBeanResource(beanResource());
    }
    
    @Bean
    public OtherBeanResource otherBeanResource() {
        return new OtherBeanResource(beanResource());
    }
}
```

* 이렇게 빈을 생성하는 메서드를 여러 번 호출하였다면 불필요하게 여러 개의 빈이 생성된다.
* 스프링은 이런 문제를 방지하고자 `@Configuration`이 있는 클래스를 객체로 생성할 때 CGLib를 사용해 프록시 패턴을 적용한다.
  * 따라서 `@Configuration` 내에서 `@Bean`이 있는 메서드를 여러 번 호출해도 **항상 동일한 객체를 반환하여 싱글톤이 보장**된다.
* 다음과 같은 프록시 객체가 구현되겠다고 이해할 수 있다.

```java
@Configuration
public class BeanResourceConfigProxy extends BeanResourceConfig {
    private Object source;
    
    @Override
    public BeanResource beanResource() {
        if (source == null) {
            source = super.beanResource();
        }
        
        return source;
    }
    
    @Override
    public SecondBeanResource secondBeanResource() {
        return super.secondBeanResource();
    }
    
    @Override
    public OtherBeanResource otherBeanResource() {
        return super.otherBeanResource();
    }
}
```
