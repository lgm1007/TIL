# Kafka 주요 설정
* 카프카 주요 설정들에 대해 정리한 문서이다.
* 카프카를 보다 정확하게 사용하기 위해서는 설정에 대한 명확한 이해가 필요하다.

### Kafka 공통 설정
|설정|설명|
|---|---|
|bootstrap.servers|컨슈머가 연결할 브로커의 정보를 입력한다.|

### Kafka 컨슈머 주요 설정
|설정|설명|
|---|---|
|heartbeat.interval.ms|컨슈머의 상태가 activate임을 의미한다. <br/>정상 동작을 위해서는 `sesson.timeout.ms`의 1/3 수준으로 설정해야 한다.|
|max.poll.interval.ms|컨슈머가 poll을 한 후 얼마나 기다려야 하는지를 의미한다. <br/>한 번 poll한 컨슈머가 해당 설정만큼 다시 poll하지 않으면 리밸런싱이 일어난다. <br/>이는 파티션 무한 점유를 막기 위한 설정으로, 로직이 너무 오래 걸려서 리밸런싱이 되는 경우 중복 컨슈밍되는 문제가 발생할 수 있어 적절한 설정이 필요한 옵션|
|max.partition.fetch.bytes|파티션 당 가져올 수 있는 최대 크기|
|session.timeout.ms|해당 주기마다 컨슈머가 종료된 것인지 판단한다. <br/>이 시간 전까지 `heartbeat`를 보내지 않으면 컨슈머는 종료된 것으로 간주하고 컨슈머 그룹에서 제외하고 리밸런싱을 시작한다.|
|enable.auto.commit|백그라운드로 주기적으로 오프셋을 커밋한다.|
|auto.offset.reset|카프카에서 초기 오프셋이 없거나 현재 오프셋이 더 이상 존재하지 않을 경우 다음 옵션으로 reset한다.|
|fetch.max.bytes|한 번의 가져오기 요청으로 가져올 수 있는 최대 크기이다.|
|group.instance.id|컨슈머의 고유한 식별자이다. <br/>만약 이 값이 설정되어 있다면 static 멤버로 간주되어 불필요한 리밸런싱을 하지 않는다.|
|isolation.level|트랜잭션 컨슈머에서 사용되는 옵션이다. <br/>read_uncommitted는 기본값으로 모든 메시지를 읽는다. <br/>하지만 read_committed는 트랜잭션이 완료된 메시지만 읽는다.|
|max.poll.records|한 번의 `poll()` 요청으로 가져오는 최대 메시지 수이다.|
|partition.assignment.strategy|파티션 할당 전략이며, 기본값은 `range`이다.|
|fetch.max.wait.ms|`fetch.min.bytes`에 의해 설정된 데이터보다 적은 경우 요청에 대한 응답을 기다리는 최대 시간이다.|

### Kafka 프로듀서 주요 설정
|설정|설명|
|---|---|
|request.timeout.ms|요청 응답에 대한 클라이언트의 최대 대기 시간이다. <br/>타임아웃 시간 동안 응답을 받지 못하면 요청을 다시 보냄|
|delivery.timeout.ms|`send()` 메서드를 호출하고 성공과 실패를 결정하는 상한 시간이다. <br/>브로커로부터 ack를 받기 위해 대기하는 시간이며, 실패 시 재전송에 허용된 시간 <br/>복구할 수 없는 오류가 발생하거나 재시도 횟수를 다 소모하면 `delivery.timeout.ms` 설정 시간보다 먼저 에러를 낼 수 있음 <br/> `delivery.timeout.ms` 값은 `linger.ms`의 합보다 같거나 커야 함|
|acks|프로듀서가 토픽의 리더 측에 메시지를 전송한 후 전송 완료를 결정하는 옵션이다. <br/>`0`: 빠른 전송을 보장하나, 일부 메시지의 손상 가능성 존재 <br/>`1`: 리더가 메시지를 받았는지 확인. 모든 팔로워를 전부 확인하지는 않으며, 대부분 이 설정값을 사용함 <br/>`all`: 팔로워까지 메시지를 받았는지 여부 확인. 느릴 수 있지만 하나 이상의 팔로워가 있는 한 메시지는 손실되지 않음| 
|batch.size|프로듀서가 동일한 파티션으로 전송할 데이터의 배치 크기이다.|
|linger.ms|배치 크기에 도달하지 못한 상황에서 `linger.ms`로 설정한 제한 시간에 도달했을 때 메시지를 전송한다.|
|max.request.size|요청할 수 있는 최대 bytes 크기이다. <br/>거대한 요청을 피하기 위해 프로듀서가 한 배치에 보낼 수 있는 사이즈를 제한함 <br/>압축되지 않은 배치의 최대 사이즈 <br/> 서버에서는 별도의 배치 사이즈 설정을 가지고 있음|
|buffer.memory|프로듀서가 카프카 서버로 데이터를 보내기 위해 잠시 대기 (배치 전송이나 딜레이 등)할 수 있는 전체 메모리 byte이다.|
|compression.type|프로듀서가 메시지 전송 시 선택할 수 있는 타입이다. `none`, `gzip`, `snappy`, `lz4`, `zstd` 중 원하는 타입 선택|
|enable.idempotence|설정을 `true`로 하는 경우 중복 없이 전송 가능하다. <br/>이와 동시에 `max.in.flight.requests.per.connection`은 5 이하, `retries`는 0 이상, `acks`는 `all`로 설정해야 한다.|
|max.in.flight.requests.per.connection|하나의 커넥션에서 프로듀서가 최대한 ack 없이 전송할 수 있는 요청 수이다. <br/>메시지의 순서가 중요하다면 `1`로 설정할 것을 권장하나 성능은 다소 떨어짐|
|retries|일시적인 오류로 인해 전송에 실패한 데이터를 다시 보내는 횟수이다.|
|transactional.id|정확히 한 번 전송을 위해 사용하는 옵션이다. <br/>동일한 TransactionalId에 한해 정확히 한 번 전송을 보장 <br/>`enable.idempotence` 설정을 `true`로 해야 함|

### Kafka 리스너 주요 설정
|설정|설명|
|---|---|
|concurrency|2개 이상의 컨슈머 스레드를 실행하고 싶을 경우 사용하는 옵션이다. <br/>`concurrency` 옵션값에 해당하는 만큼 컨슈머 스레드를 만들어 병렬 처리함 <br/>`concurrency` 개수의 기준은 파티션의 개수와 맞추는 것이 가장 효과적|
|ack-mode|`record`: 레코드를 처리한 후 리스너가 반환할 때 커밋 <br/>`batch`: poll() 메서드로 호출한 레코드가 모드 처리된 이후 커밋 <br/>`time`: ackTime 만큼 지난 이후 커밋, 시간 간격을 선언하는 ackTime 옵션을 설정해야 함 <br/>`count`: ackCount로 설정된 개수 만큼 레코드가 처리된 이후 커밋, 레코드 개수를 선언하는 ackCount 옵션을 설정해야 함 <br/>`count_time`: `count`, `time` 중 맞는 조건이 나오면 커밋 <br/>`manual`: Acknowledgement.acknowledge() 메서드가 호출되면 다음 번 poll() 메서드 호출 시 커밋, 매번 acknowledge()를 호출하면 `batch` 옵션과 동일, AcknowledgingMessageListener, BatchAcknowledgingMessageListener를 사용해야 함 <br/>`manual_immediate`: Acknowledgement.acknowledge()가 호출되면 커밋, AcknowledgingMessageListener, BatchAcknowledgingMessageListener를 사용해야 함|

### Kafka 설정 예제
* application.yml 형식 예제
```yaml
spring:
  kafka:
    bootstrap-servers: 111.11.111.11:9092
    consumer:
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
      max-poll-records: 1
      group-id: \${spring.application.name}
      enable-auto-commit: false
      heartbeat-interval: 20000
      properties:
        request.timeout.ms: 180000
        max.poll.interval.ms: 30000
        session.timeout.ms: 60000
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
      acks: all
      properties:
        request.timeout.ms: 180000
    properties:
      spring.json.trusted.packages: com.example.*
      request.timeout.ms: 180000
    listener:
      concurrency: 1
      ack-mode: manual_immediate
```
