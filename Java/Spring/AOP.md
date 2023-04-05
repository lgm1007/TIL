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
### AOP 적용 방법
1. **@Aspect 어노테이션**
    * Aspect 클래스를 작성하고 @Aspect 어노테이션을 클래스에 붙여준다.
    * @Before, @After, @Around 등의 Advice 어노테이션을 사용하여 메서드 실행 전/후, 예외 발생 시 등에 실행될 코드를 작성한다.
    * 이후 @Pointcut 어노테이션을 사용하여 Advice가 실행될 Join point를 지정한다.
    * ```java
      @Aspect
      @Component
      public class LoggingAspect {
        @Before("execution(public * com.example.demo.service.*.*(..))")
        public void logBefore(JoinPoint joinPoint) {
            System.out.println("Method " + joinPoint.getSignature().getName() + " is called.");
        }
      }
      ```
2. Proxy 기반 AOP
    * Spring 에서는 Proxy 기반 AOP를 사용한다.
    * Proxy는 Target 객체와 Interface를 이용하여 생성된다.
    * Proxy 객체는 Target 객체의 메서드 호출을 가로채 Advice를 실행하고, 그 결과를 반환한다.
    * ```java
      public interface UserService {
        void addUser(User user);
        void deleteUser(int userId);
        List<User> getUsers();    
      }
      
      public class UserServiceImpl implements UserService {
        private final List<User> users = new ArrayList<User>();
      
        public void addUser(User user) {
            users.add(user);
        }
      
        public void deleteUser(int userId) {
            for (User user : users) {
                if (user.getId() == userId) {
                    users.remove(user);
                    break;
                }
            }
        }
      
        public List<User> getUsers() {
          return users;
        }
      }

      public class UserServiceLoggingProxy implements UserService {
        private final UserService userService;
      
        public UserServiceLoggingProxy(UserService userService) {
            this.userService = userService;
        }
      
        @Override
        public void addUser(User user) {
            System.out.println("Before adding user...");
            userService.addUser(user);
            System.out.println("After adding user...");
        }
      
        @Override
        public void deleteUser(int userId) {
            System.out.println("Before deleting user...");
            userService.deleteUser(userId);
            System.out.println("After deleting user...");
        }
      
        @Override
        public List<User> getUsers() {
            System.out.println("Before getting user...");
            List<User> users = userService.getUsers();
            System.out.println("After getting user...");
            return users;
        }
      }
      
      public class Main {
        public static void main(String[] args) {
          UserService userService = new UserServiceImpl();
          UserService proxy = new UserServiceLoggingProxy();
      
          proxy.addUser(new User(1, "Apple"));
          proxy.addUser(new User(2, "Banana"));
      
          proxy.getUsers();
      
          proxy.deleteUser(1);
        }
      }
      ```
      * `UserServiceLoggingProxy` 클래스를 사용하여 인터페이스의 구현체(`UserServiceImpl`)을 호출하면서 특정한 메서드 호출 전/후에 로그를 출력할 수 있게 된다.


