# Transactional Outbox Pattern
* EDA (Event Driven Architect) 서비스는 대개 메시지 브로커를 통해 다양한 메시지 (이벤트) 를 publish 하고, 그 이벤트에 해당하는 작업을 비동기로 처리하는 방식으로 시스템이 동작한다.
* 이러한 서비스에서 **특정 DB 상태를 변경하는 트랜잭션과 함께 이벤트를 발행해야 하는 경우**가 종종 있다.
* 그 때 **이벤트 발행과 트랜잭션 커밋 시점 문제**를 고민해야 한다.
  * 보통 애플리케이션 로직 상 트랜잭션 완료 전에 이벤트를 발행한다.
  * 이는 만약 비즈니스 로직 중 예외가 발생하거나 DB 쿼리가 특정 이유 때문에 롤백이 되는 경우, **트랜잭션이 성공하지 못했는데 메시지가 발행되는 경우**가 생길 수 있어 시스템 안정성을 보장할 수 없게 된다.
    * DB 트랜잭션에서는 쿼리들은 원자적으로 실행되지만, 메시지 브로커는 원자적인 처리가 불가능하다.
* 위와 같은 문제를 해결하기 위한 방법 중 하나가 **Transactional Outbox Pattern** 이다.

### Transactional Outbox Pattern 철학
* 최종적 일관성 (Eventual Consistency)
* 메시지 전달 보장 (Guaranteed Message Delivery)
* 아래와 같은 **분산 시스템의 핵심 과제**를 해결
  1. **데이터베이스 트랜잭션과 메시징 시스템 간의 불일치** 해결
      * DB 업데이트와 메시지 발행이 별도의 시스템 또는 프로세스에서 일어나므로, 둘 중 하나만 성공하고 다른 하나는 실패할 수 있는 문제
  2. 시스템 장애 시 **메시지 손실 방지**
      * 메시지 브로커로 메시지를 보내다 실패하면 해당 메시지가 영구적으로 손실될 수 있는 문제
  3. **비즈니스 로직과 메시징 로직 분리**
      * 비즈니스 로직과 메시지 발생 로직이 함께 존재하면 코드 복잡도가 올라감으로, 메시지 발행을 별도의 프로세스로 분리하여 관심사를 명확히 구분

### Transactional Outbox Pattern 동작

![transactional_outbox_pattern](https://microservices.io/i/patterns/data/ReliablePublication.png)

* Oubox 테이블을 두고 DB 트랜잭션의 일부로 메시지를 저장함으로써 DB 쿼리와 메시지 브로커 작업을 원자적으로 만든다.
* 그 다음 별도의 애플리케이션이나 프로세스로 테이블에 저장된 메시지를 읽어 메시지 브로커에 전송하는 방식
* 실패 시 완료될 때까지 다시 시도할 수 있다.
* Outbox Pattern은 적어도 한 번 이상 (at-least once) 메시지가 성공적으로 전송되었는지 확인할 수 있다.

#### Transactional Outbox Pattern의 구성 요소
* **Sender** : 메시지를 보내는 서비스
* **Database** : 엔티티 및 메시지 Outbox를 저장하는 데이터베이스
* **Message Outbox**
  * RDBMS인 경우 : 보낼 메시지를 저장하는 테이블
  * NoSQL인 경우 : 각 데이터베이스 record의 프로퍼티
* **Message Relay** : Outbox에 저장된 메시지를 메시지 브로커로 보내는 서비스

### Transactional Outbox Pattern을 구현하는 방식
#### Polling Publisher
![polling_publisher](https://velog.velcdn.com/images/eastperson/post/e4787feb-f33b-460e-9b76-b8a688197c44/image.png)

* RDBMS를 사용하는 애플리케이션에서 Outbox 테이블에 저장된 메시지를 발행하는 방법
  * 테이블을 Polling해서 미발행된 메시지를 조회하는 방식
  * Message Relay는 이렇게 조회한 메시지를 메시지 브로커에 전달
  * 추후 Outbox 테이블에서 메시지 삭제
* Polling 방식은 DB 규모가 작을 경우 사용할 수 있는 간단한 방식이다.
* 자주 Polling 하면 비용이 발생하고, NoSQL의 경우 쿼리 능력에 따라 사용 가능 여부가 갈린다.

#### Transactional Log Tailing Pattern
![Transactional_log_tailing](https://microservices.io/i/patterns/data/TransactionLogMining.png)

* Message Relay로 DB 트랜잭션 로그 (커밋 로그)를 테일링하는 방법
  * 애플리케이션에서 커밋된 업데이트는 각 DB의 트랜잭션 로그 항목 (Log Entry) 으로 남는다.
  * 트랜잭션 로그 마이너 (Transaction Log Miner)로 트랜잭션 로그를 읽어 변경분을 하나씩 메시지로 메시지 브로커에 발행하는 방식
  * MySQL의 경우엔 binlog, PostgreSQL의 경우엔 WAL, Oracle의 경우엔 redolog를 활용하여 변경 사항을 읽는다.
* 구현 난이도가 높아 관련 툴을 사용하여 구현하는 경우가 많은 방법
  * 디비지움 (Debezium), 링크드인 데이터 버스 (LinkedIn Databus), DynamoDB 스트림즈, 트램 등

### Transactional Outbox Pattern의 목적
1. **데이터 일관성 보장**
    * 메인 트랜잭션과 메시지 저장이 원자적으로 이뤄진다.
    * DB 상태와 발행된 메시지 간 불일치 해결
2. **메시지 손실 방지: 높은 신뢰성**
    * 모든 메시지가 DB에 저장되기 때문
3. **순서 보장**
    * DB에 저장된 순서대로 메시지를 발행함으로써 이벤트의 시간적 순서 보장
4. **재시도 메커니즘**
    * Outbox 테이블을 주기적으로 Polling 하여 미발행 메시지를 재시도할 수 있다.
5. **시스템 복잡도 관리**
    * 비즈니스 로직과 메시지 발행 로직을 분리하여 시스템의 복잡도를 낮춘다.
    * 각 역할의 책임을 명확히 하여 유지보수성을 높인다.
