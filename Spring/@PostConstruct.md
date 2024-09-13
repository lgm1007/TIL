# @PostConstruct
###  설명
* `@PostConstruct`은 의존성 주입이 완료된 후에 수행되어야 하는 메서드에 사용하는 어노테이션이다.
* 해당 어노테이션이 선언된 메서드는 **다른 리소스에서 호출되지 않아도 수행**된다.
* 생성자보다 늦게 호출된다.
  1. 생성자 호출
  2. 의존성 주입 (`@RequiredArgsConstructor` or `@Autowired`)
  3. `@PostConstruct` 호출

### 사용 이유
* 생성자가 호출될 때 bean은 초기화 전으로, `@PostConstruct`를 사용하면 **bean이 초기화 됨과 동시에 의존성을 확인**할 수 있다.
* bean의 생명 주기에서 **단 한 번만 수행**되므로 초기화 메서드에 선언하면 여러 번 초기화되는 문제를 방지할 수 있다.

### 사용 예제
```java
@Service
@RequiredArgsConstructor
public class testService {
    // ...
    
    private final Map<String, Object> serviceSetups = new HashMap<>();
    
    @PostConstruct
    public void setup() {
        serviceSetups.put("service", Service.TEST);
        serviceSetups.put("applicationType", ApplicationType.PROTOTYPE);
        serviceSetups.put("message", "This setup is occured in PostConstruct.");
    }
}
```


