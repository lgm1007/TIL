# DI (Dependency Injection)
* 의존성 주입, 객체 간의 **결합도를 낮추고 유연성을 높이**기 위한 방법
* 인터페이스를 두 객체 사이에 둬서 클래스 레벨에서는 의존관계가 고정되지 않도록 런타임 시에 관계를 동적으로 주입하는 방식

### DI가 필요한 이유
* 기본적으로 객체 지향 프로그래밍에서는 한 객체가 다른 객체를 사용하기 위해서는 해당 객체를 **직접 생성**하여야 한다.
  * ```java
    public class MyClass {
        private AClass aClass;
    
        public MyClass() {
            this.aClass = new AClass();
        }
    }
    ```
  * 이런 방식은 코드의 유연성과 재사용성을 낮추고, 객체 간의 결합도가 높아진다는 문제가 있다.
    * 만약 위 예제의 `MyClass`에서 `AClass` 가 아닌 `BClass`가 필요해진다면 `MyClass`의 생성자에 변경이 필요하다.
      * 유연성이 떨어짐
* 위와 같은 문제를 해결하기 위해서는 우선 **다형성**이 필요하다.
  * ```java
    public interface Product {
    
    }
    
    public class AClass implements Product {
    
    }
    ```
  * `AClass`, `BClass` 등 여러 객체들을 하나로 표현해주는 `Product` 라는 인터페이스가 필요하며 각 클래스에서 `Product` 인터페이스를 구현한다.
  * ```java
    public class MyClass {
        private Product product;
    
        public MyClass(Product product) {
            this.product = product;
        }
    }
    ```
 * 그리고 `MyClass`와 `AClass` 가 결합되어 있는 부분을 제거해주는데, 이를 위해서 외부에서 `Product`를 주입받아야 한다.
   * 이로써 `MyClass`에서 구체 클래스에 의존하지 않는다.
 * `MyClass`에서 `Product` 객체를 주입하기 위해서는 애플리케이션 실행 시점에 필요한 객체(빈)를 생성해야 하며, 의존성이 있는 두 객체를 연결하기 위해 한 객체를 다른 객체로 주입시켜야 한다.
   * 이렇게 주입시켜주는 역할을 Spring 이라는 DI 컨테이너가 지원해준다.
 * Spring은 **특정 위치부터 클래스를 탐색**하고, 객체를 만들며 **객체들의 관계까지 설정**해준다.
   * **제어의 역전**([IoC](./IoC.md)) 개념
     * 사용자는 **수동으로 주입받는 객체**를 사용하기 때문

### DI 컨테이너의 동작 원리
#### Spring 프레임워크
1. **의존성 주입 설정**
    * 먼저, 애플리케이션의 설정 파일 (XML 또는 JavaConfig)이나 어노테이션을 통해 의존성 주입을 설정한다. 이 때 주입할 객체의 타입과 위치를 명시한다.
2. **의존성 해결**
    * DI 컨테이너는 애플리케이션이 실행될 때 필요한 객체들의 의존성을 해결한다. 이를 위해 설정된 정보를 바탕으로 필요한 객체들을 생성하고, 필요한 의존성을 주입한다.
3. **의존성 관리**
    * DI 컨테이너는 생성된 객체들의 라이프사이클을 관리하고, 필요한 경우 해당 객체들을 재사용한다. 이를 통해 애플리케이션의 메모리 사용을 최적화하고 성능을 향상시킨다.

#### Spring Boot
1. **Component Scan**
    * Spring Boot 애플리케이션은 클래스 경로 내에 있는 모든 컴포넌트를 자동으로 검색한다.
    * 이를 통해 `@Component`, `@Service`, `@Repository`, `@Controller` 등의 어노테이션으로 마킹된 클래스들을 찾는다.
2. **의존성 주입**
    * Component Scan을 통해 찾은 클래스들을 IoC 컨테이너에 등록하여 Bean으로 식별한다. 그리고 필요한 의존성을 자동으로 주입한다.
3. **자동 구성** (auto-configuration)
    * Spring Boot는 클래스패스 상의 설정 및 의존성에 따라 애플리케이션을 자동으로 구성한다.
    * 이를 통해 DI 컨테이너가 자동으로 빈을 생성하고 의존성을 주입할 수 있다.
4. **외부 설정**
    * Spring Boot는 외부 설정 파일 (properties, yaml 파일)을 통해 빈의 생성 및 의존성 주입을 구성할 수 있다.
    * 이를 통해 애플리케이션의 설정을 외부로 분리하여 유지보수 및 환경별 설정 관리를 용이하게 한다.

### DI 방법
1. 생성자 주입 (Constructor Injection)
   * 생성자를 통해 의존성을 주입하므로, 객체 생성 시점에 필요한 의존성이 모두 주입되어 안정적인 객체 생성이 가능하다.
   * 예제 소스
```java
public class MyClass {
    private MyDependency myDependency;
    
    public MyClass(MyDependency myDependency) {
        this.myDependency = myDependency;
    }
}
```
2. @RequiredArgsConstructor 어노테이션을 활용한 생성자 주입
   * 클래스 내부의 final 필드에 대해서 생성자를 작성해주는 어노테이션 ([Lombok 어노테이션](./Lombok.md))
   * @Autowired 어노테이션을 생략할 수 있어 코드가 간결해진다.
   * 예제 소스
```java
@RequiredArgsConstructor
public class MyClass {
    private final MyDependency myDependency;
    private final MyOtherDependency myOtherDependency;
}
```
3. Setter 메서드 주입 (Setter Injection)
   * 클래스 내부 Setter 메서드를 이용해 의존성을 주입하는 방법
   * Setter 메서드를 통해 의존성을 주입하므로, 객체 생성 이후에도 의존성을 변경할 수 있다.
   * Setter 메서드를 일일이 작성해줘야 하기 때문에 코드가 길어질 수 있다.
   * 예제 소스
```java
public class MyClass {
    private MyDependency myDependency;
    
    public void setMyDependency(MyDependency myDependency) {
        this.myDependency = myDependency;
    }
}
```
4. 필드 주입 (Field Injection)
   * 클래스 내부의 필드를 직접 주입하는 방법
   * 코드가 간결해진다.
   * 객체 생성 이후에는 의존성을 변경할 수 없다.
   * 예제 소스
```java
public class MyClass {
    @Autowired
    private MyDependency myDependency;
}
```

