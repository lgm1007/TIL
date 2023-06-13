# @Transactional
### 트랜잭션
* DB 트랜잭션이란, DB 관리 시스템 또는 유사한 시스템에서의 **상호작용의 단위**이다.
	* 단위 : **더 이상 쪼개질 수 없는 최소의 연산**
* *결제* 작업을 트랜잭션 예시로 들 경우
	* 결제는 다른 사람과 독립적으로 이루어지며 과정 중에 다른 연산이 끼어들 수 없다. 오류가 생긴 경우 연산을 취소하고 원래대로 되돌린다. 성공할 경우 결과를 반영한다.
	* 트랜잭션의 원칙 : **ACID** (원자성, 일관성, 고립성, 지속성)
### @Transactional 어노테이션
* 클래스나 메서드에 붙일 경우, 해당 범위 내 메서드가 트랜잭션이 되도록 보장해준다.
	* **선언적 트랜잭션** : 직접 객체를 만들 필요 없이 선언만으로도 관리를 용이하게 해줌
* 예제 코드
```java
@RequiredArgsConstruct
@Service
public class MemberService {
	private final MemberRepository memberRepository;
	
	@Transactional(readOnly = true)
	public List<MemberResponseEntity> getMembers() {
		return bookRepository.findAll();
	}
}
```
* `getMembers()` 메서드는 아래와 같은 속성을 가진다.
	* 연산이 고립되어 다른 연산과의 혼선으로 인해 잘못된 값을 가져오는 경우가 방지된다.
    * 연산의 원자성이 보장되어 연산이 도중에 실패할 경우 변경사항이 반영되지 않는다.
* 따라서 해당 메서드가 실행되는 도중 메서드 값을 수정 또는 삭제하려는 시도가 들어와도 값의 신뢰성이 보장된다.
### @Transactional 작동 원리 및 프로세스
* @Transactional 어노테이션이 클래스 또는 메서드에 붙을 경우 Spring이 해당 메서드에 대하여 프록시를 만든다.
	* 프록시 패턴
    * 어떤 코드를 감싸면서 추가적인 연산을 수행하도록 강제하는 패턴
* 트랜잭션의 경우 시작과 연산 종료 시의 커밋 과정이 필요하므로, 프록시를 생성해 해당 메서드의 앞뒤에 트랜잭션의 시작과 끝을 추가하는 것
* 또한 Spring 컨테이너는 **트랜잭션 범위의 영속성 컨텍스트 전략**을 기본으로 사용한다.
	* 서비스 클래스에서 @Transactional 어노테이션을 사용하면, 해당 코드 내의 메서드를 호출할 때 영속성 컨텍스트가 생긴다는 의미
### @Transactional 어노테이션 옵션
1. `isolation`
	* **동시에 여러 사용자가 데이터에 접근할 때 어디까지 허용할지**에 대하여 정하는 옵션
    * 트랜잭션의 격리 수준과 데이터의 일관성은 비례
    * @Transactional에서는 5가지 `isolation` 옵션 제공
      1. `DEFAULT`
         * 기본 격리 수준
      2. `READ_UNCOMMITTED`
         * 한 트랜잭션이 처리 중인 커밋되지 않은 데이터를 다른 트랜잭션이 접근 가능하도록 함
         * DB에 커밋되지 않은, 존재하지 않는 데이터를 읽는 현상을 Dirty Read 라고 함
         * Dirty Read가 가능하기 때문에 잘못된 데이터를 읽을 수도 있음
      3. `READ_COMMITTED`
	      * 커밋된 데이터만 접근 가능하도록 함
          * Non-Repeatable Read 현상이 발생할 수 있음
            * 트랜잭션에서 조회한 데이터가 트랜잭션이 끝나기 전의 다른 트랜잭션에 의해 변경되면 다시 읽었을 때 다른 값이 읽히며 데이터간 불일치가 발생하는 현상
      4. `REPEATABLE_READ`
	      * 하나의 트랜잭션은 하나의 스냅샷만 사용하도록 함
          * A 트랜잭션이 시작하고 처음 조회한 데이터의 스냅샷을 저장, 이후 동일한 쿼리를 호출하면 스냅샷에서 데이터를 가져옴
          * Phantom Read 현상이 발생할 수 있음
            * 다른 트랜잭션에서 수행한 작업에 의해 안보였던 데이터가 보이는 현상
      5. `SERIALIZABLE`
	      * 가장 단순하고 엄격한 격리 수준
          * 순차적으로 트랜잭션을 진행시키며 읽기 작업에도 잠금을 걸어 여러 트랜잭션이 동시에 같은 데이터에 접근하지 못하게 함
          * 가장 안전하지만 성능 저하가 발생하기 때문에 극도의 안정성을 필요로 하지 않을 경우 자주 사용하지 않음
2. `propagation`
	* 현재 진행중인 트랜잭션 (부모 트랜잭션) 이 존재할 때 새로운 트랜잭션 메서드를 호출할 경우 어떤 정책을 사용할 지 정의하는 속성
    * @Transactional에서는 7가지 `propagation` 옵션 제공
      1. `REQUIRED`
         * 기본 값으로, 부모 트랜잭션이 존재할 경우 참여하고 없는 경우 새 트랜젹션 시작
      2. `SUPPORTS`
	      * 부모 트랜잭션이 존재할 경우 참여하고 없는 경우 non-transactonal 상태로 실행
      3. `MANDATORY`
	      * 부모 트랜잭션이 존재할 경우 참여하고 없는 경우 예외 발생
      4. `REQUIRES_NEW`
	      * 부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션이 생성
      5. `NOT_SUPPORTED`
	      * non-transactional 상태로 실행하며 부모 트랜잭션이 존재하는 경우 일시정지
      6. `NEVER`
	      * non-transactional 상태로 실행하며 부모 트랜잭션이 존재하는 경우 예외 발생
      7. `NESTED`
	      * 부모 트랜잭션과는 별개로 중첩된 트랜잭션 생성
          * 부모 트랜잭션의 커밋과 롤백에는 영향을 받지만 자신의 커밋과 롤백은 부모 트랜잭션에 영향 주지 않음
          * 부모 트랜잭션이 존재하지 않는 경우 새로운 트랜잭션 생성 (`REQUIRED`)
          * DB가 SAVEPOINT를 지원해야 사용 가능 (Oracle)
          * `JpaTransactionManager` 에서는 지원하지 않음
3. `readOnly`
	* 기본값은 `false`며 `true`로 세팅하는 경우 트랜잭션을 읽기 전용으로 변경함
    * **성능 향상**을 위해 사용하거나 읽기 외 다른 동작을 방지하기 위해 사용하기도 함
4. `rollbackFor`
	* 사용법: `@Transactional(rollbackFor = {IOException.class, ClassNotFoundException.class})`
    * 사용 시 `@Transactional(rollbackFor = IOException.class)` 처럼 Exception을 하나만 지정한다면 중괄호 생략 가능
    * 특정 Exception 발생 시 데이터를 커밋하지 않고 롤백하도록 설정
5. `timeout`
	* 사용법: `@Transactional(timeout = 3)`
    * 지정된 시간 내에 해당 메서드 수행이 완료되지 않는 경우 `JpaSystemException` 발생
    * `JpaSystemException`은 `RuntimeException`을 상속받기 때문에 데이터는 롤백 처리됨
    * 초 단위로 지정할 수 있으며 -1 값으로 지정할 경우 timeout을 지원하지 않음
### 왜 `@Transactional(readOnly = true)` 설정일 때 성능이 향상되는가?
* JPA의 영속성 컨텍스트가 수행하는 **변경 감지**(Dirty Checking)와 관련이 있다.
  * **변경 감지?**
    * 영속성 컨텍스트는 Entity 조회 시 초기 상태에 대한 스냅샷을 저장한다.
    * 트랜잭션이 Commit 될 때, 초기 상태의 정보를 가지는 스냅샷과 Entity의 상태를 비교하여 변경된 내용에 대해 update query 로 쓰기 지연 저장소에 저장한다.
    * 그 후 일괄적으로 쓰기 지연 저장소에 저장되어 있는 SQL query를 flush 하고 데이터베이스의 트랜잭션을 Commit 함으로서, 우리가 **update와 같은 메서드를 사용하지 않고도 Entity의 수정**이 이루어진다. 이를 **변경 감지**라고 한다.
  * `readOnly = true` 로 설정하게 되면 스프링 프레임워크는 JPA의 세션 **플러시 모드를 수동**으로 설정해, 사용자가 수동으로 `flush()` 호출하지 않으면 flush가 수행되지 않게 한다.
    * 트랜잭션 내에서 강제로 `flush()`를 호출하지 않으면, 수정 내역에 대해 DB에 적용되지 않는다.
    * 이로 인해 트랜잭션 Commit 시 영속성 컨텍스트가 **자동으로 flush 되지 않으므로** 조회용으로 가져온 **Entity의 예상치 못한 오류를 방지**할 수 있다.
  * 또한 `readOnly = true` 를 설정하게 되면 JPA는 해당 트랜잭션 내에서 조회하는 Entity는 조회용임을 인식하고 **변경 감지를 위한 스냅샷을 따로 보관하지 않으므로 메모리가 절약**되는 성능상 이점 또한 존재하게 된다.
