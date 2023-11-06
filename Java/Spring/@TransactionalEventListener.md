# @TransactionalEventListener
* Spring 프레임워크에서 제공하는 어노테이션으로, 트랜잭션 이벤트를 처리하기 위해 사용한다.
* 트랜잭션 이벤트에 대한 리스너를 정의할 때 사용되며, 특정 이벤트가 발생할 때 **트랜잭션 커밋 또는 롤백 이후**에 이벤트를 처리할 수 있다.

### 속성
1. **classes**
    * 이벤트 클래스를 나타내며, 이벤트를 지정하여 처리할 수 있다.
```java
/**
 *  OrderCreatedEvent와 OrderUpdatedEvent 에 대한 이벤트를 처리하는 메서드
 */
@TransactionalEventListener(classes = {OrderCreatedEvent.class, OrderUpdatedEvent.class})
public void handleOrderEvent(OrderEvent event) {
    // 주문 이벤트 처리
}
```

2. **condition**
    * 조건을 지정하여 이벤트 처리
```java
@TransactionalEventListener(condition = "#event.amount > 1000")
public void handleHighAmountOrderEvent(OrderEvent event) {
	// 주문 금액이 1000 이상인 이벤트 처리
}
```

3. **fallbackExecution**
    * 트랜잭션이 없는 경우에 이벤트를 처리할 지 여부
```java
@TransactionalEventListener(fallbackExecution = true)
public void handleOrderEvent(OrderEvent event) {
    // 트랜잭션이 없는 경우에도 주문 이벤트 처리
}
```

4. **phase**
    * 이벤트 핸들링의 타이밍을 나타내며, `BEFORE_COMMIT`, `AFTER_COMMIT`, `AFTER_ROLLBACK` 등이 있다.
    * 디폴트 값은 `AFTER_COMMIT`
    * 만약 이벤트 리스너에서 추가로 DB 작업을 진행해야 하는 경우 `Transactional(propagation = Propagation.REQUIRES_NEW)`
      * 트랜잭션을 이어받지 않고 새로운 트랜잭션을 시작하겠다는 의미
```java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
public void handleAfterCommitEvent(OrderEvent event) {
	// 트랜잭션 커밋 이후에 이벤트 처리
}

@TransactionalEventListener(phase = TransactionPhase.AFTER_ROLLBACK)
public void handleAfterCommitEvent(OrderEvent event) {
		// 트랜잭션 롤백 이후에 이벤트 처리
}

@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void handleAfterCommitEvent(OrderEvent event) {
	Order order = event.getOrder();
	
	// DB 작업 로직
}
```

### 사용 예제
* 주문 이벤트와 주문 실패 이벤트 발행
```java
@Service
@RequiredArgsConstructor
public class OrderService {
	
	private final OrderRepository orderRepository;
	
	private final ApplicationEventPublisher publisher;
	
	@Transactional
    public void createOrder(int orderId) {
		Order order = orderRepository.findById(orderId);
		
		// 주문 생성 로직 ...
        
        // 주문 생성 이벤트 발행
        publisher.publishEvent(new OrderCreatedEvent(order));
    }
	
	@Transactional
    public void failOrder(int orderId) {
		Order order = orderRepository.findById(orderId);
		
		// 주문 실패 시 로직 ...
        
        // 주문 실패 이벤트 발행
        publisher.publishEvent(new OrderFailedEvent(order));
    }
}
```

* 주문 이벤트 정의
```java
@Component
@RequiredArgsConstructor
public class OrderEventListener {
	
	@TransactionalEventListener(phase = TransactionalPhase.AFTER_COMMIT)
	public void handleOrderCreatedEvent(OrderCreateEvent event) {
		// 트랜잭션 커밋 이후에 주문 생성 이벤트 처리
    }
	
	@TransactionalEventListener(phase = TransactionalPhase.AFTER_ROLLBACK)
    public void handleOrderFailedEvent(OrderFailedEvent event) {
		// 트랜잭션 롤백 이후에 주문 실패 이벤트 처리
    }
}
```
