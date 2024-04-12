# JPA 잠금(Lock)
### 낙관적 잠금 (Optimistic Lock)
* 낙관적 잠금은 데이터 갱신 시 충돌이 발생하지 않을 것이라고 낙관적으로 보고 잠금을 거는 기법
* DB에 락을 걸기보다는 **충돌 방지**에 가깝다고 볼 수 있다.
* 동시에 데이터에 대한 여러 업데이트가 서로 간섭하지 않도록 방지하는 `version`이라는 속성을 확인하여 Entity의 변경사항을 감지하는 메커니즘
* JPA에서 낙관적 잠금을 사용하기 위해서는 Entity 내부에 `@Version` 어노테이션이 붙은 Int, Long 타입의 변수를 구현해줌으로써 구현이 가능하다.
  * `@Version` 사용 시 주의사항
    1. 각 엔티티 클래스에는 하나의 버전 속성만 있어야 한다.
    2. 여러 테이블에 매핑된 엔티티의 경우 기본 테이블에 배치되어야 한다.
    3. 버전에 명시할 타입은 `int`, `Integer`, `long`, `Long`, `short`, `Short`, `java.sql.Timestamp` 중 하나여야 한다.
* JPA는 select 시 트랜잭션 내부에 버전 속성의 갑승ㄹ 보유하고 트랜잭션이 업데이트하기 전에 버전 속성을 다시 확인한다. 그 동안 버전 정보가 변경되면 `OptimisticLockException`이 발생하고 변경되지 않으면 트랜잭션은 버전 속성을 증가시키는 업데이트를 한다.

#### 사용 예제
```java
@Entity
public class Member {
    @Id
    private Long id;
    
    private String name;
    
    private int age;
    
    @Version
    private Integer version;
}
```

### 비관적 잠금 (Pessimistic Lock)
* 트랜잭션의 충돌이 발생한다고 가정하고 우선 락을 거는 방법
* 트랜잭션 안에서 서비스 로직이 진행되어야 한다.
* 비관적 잠금 메커니즘은 **데이터베이스 수준에서 엔티티 잠금**을 포함한다.

#### 예외
* **PessimisticLockException**
  * 잠금은 Shared Lock 또는 Exclusive Lock 둘 중 하나만 획득할 수 있으며 그 락을 획득하는 데 실패하면 발생되는 예외
* **LockTimeoutException**
  * 락을 대기하다 설정해놓은 대기 시간이 초과되었을 때 발생하는 예외
* **PersistenceException**
  * NoResultException, NonUniqueResultException, LockTimeoutException, QueryTimeoutException 을 제외한 PersistenceException 예외에 대해서는 트랜잭션 롤백

#### 사용 예제
```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    @Lock(LockModeType.PESSIMISTIC_WRITE)
    Member getMemberByName(String name);
}
```

#### LockModeType 종류
1. `LockModeType.PESSIMISTIC_WRITE`
  * 일반적인 옵션, 데이터 쓰기에 잠금
  * 다른 트랜잭션에서 읽기도 쓰기도 못 한다.
2. `LockModeType.PESSIMISTIC_READ`
  * 반복 읽기만 가능하고 수정하지 않는 용도로 잠금을 걸 때 사용
  * 다른 트랜잭션에서 읽기 가능 (공유 잠금)
3. `LockModeType.PESSIMISTIC_FORCE_INCREMENT`
  * Version 정보를 사용하는 비관적 잠금
