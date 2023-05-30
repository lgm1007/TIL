# Spring Event
### Event 란?
* Spring은 내부에 **Event** 라는 메커니즘을 가지고 있다.
* Spring의 **Bean과 Bean 사이**의 데이터를 전달하는 방법 중 하나
### Event 사용 방법
* 이벤트는 이벤트를 발생시키는 `publisher`와 이벤트를 받아들이는 `listener`가 존재한다.
* 이벤트에서 데이터를 담는 `Event 모델`이 있다.
#### Event 모델
* Event 모델은 **정보**를 지니고 있다.
* 특징으로는 `ApplicationEvent`를 상속받고 super로 생성자에서 객체를 전달하며, 일반적으로 해당 객체는 `publisher` 객체를 사용한다.
```java
public class ExampleEvent extends ApplicationEvent {
    private final String name;
    private final int age;
    
    public ExampleEvent(Object object, String name, int age) {
        super(object);
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
}
```
* 위 예제는 `name`과 `age`를 전달하기 위해 만들어진 Event 모델을 보여주고 있다.
#### Event Publisher
* Event를 생성하기 위한 Bean은 `ApplicationEventPublisher` 이다.
* 보통 이를 사용하기 위해서 주입을 받아 `publishEvent` 메서드를 사용한다.
```java
@Component
public class EventTestService {
    
    private final ApplicationEventPublisher applicationEventPublisher;
    
    public EventTestService(ApplicationEventPublisher publisher) {
        this.applicationEventPublisher = publisher;
    }
    
    public void publishCustomEvent(final String message) {
        ExampleEvent exampleEvent = new ExampleEvent(this, "John", 25);
        applicationEventPublisher.publishEvent(exampleEvent);
    }
}
```
* `ExampleEvent` 이벤트를 생성하는 예제이다.
* Event 모델의 Object로 본인 객체 (`this`) 를 전달해주어 인스턴스를 만든 후 `publishEvent` 메서드의 파라미터로 넘기면 Event가 생성된다.
#### Event Listener
* `Publisher`에서 발행된 Event를 사용한다.
* `Listener`를 만들어 주기 위해선 `ApplicationListener<{EventName}>`을 구현해야 한다. 
```java
@Component
public class EventTestListener implements ApplicationListener<ExampleEvent> {
    
    @Override
    public void onApplicationEvent(ExampleEvent event) {
        // 이벤트 발생 후 수행할 동작
    }
}
```
* `ApplicationListener<{EventName}>`를 구현하여 클래스 `Listener` 클래스 생성 후 `onApplicationEvent` 메서드를 오버라이딩하여 Event를 받아들인다.
