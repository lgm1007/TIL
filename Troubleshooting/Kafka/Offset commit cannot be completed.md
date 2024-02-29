# Offset commit cannot be completed
### 에러 상황
* 직면한 에러 코드는 다음과 같다.
  * `Offset commit cannot be completed since the consumer is not part of an active group for auto partition assignment; it is likely that the consumer was kicked out of the group. `
  * Offset 커밋이 안되면서 메시지 컨슈밍이 안되는 이슈 상황

### 에러 원인
 Kafka 컨슈머 그룹에서 파티션 할당과 관련된 문제이다. <br/>

1. Kafka 컨슈머 그룹에서 파티션 할당은 컨슈머 그룹 내에서 균등하게 이루어진다.
2. 컨슈머의 `max.poll.interval.ms` 설정값은 컨슈머가 `poll()` 메서드를 호출하고 Kafka 브로커로부터 heartbeat를 받는 주기를 의미한다.
3. 컨슈머가 `max.poll.interval.ms` 값을 초과하여 `poll()` 메서드를 호출하지 못하면 컨슈머는 Kafka 브로커에게 heartbeat를 보내지 않고, Kafka 브로커는 해당 컨슈머가 죽은 것으로 판단해 해당 컨슈머의 파티션을 다른 컨슈머에게 재할당한다.
4. 컨슈머가 작업을 완료하고 다시 `poll()` 메서드를 호출하려 하는데, 이미 파티션을 잃어버렸기 때문에 `Offset commit cannot be completed since the consumer is not part of an active group for auto partition assignment` 에러가 발생한다.

### 해결 방안
1. `max.poll.interval.ms` 값 조정
    * A 작업을 처리하는데 필요한 시간이 `max.poll.interval.ms` 값을 초과하는 경우 `max.poll.interval.ms` 값을 증가시키면 된다.
    * 증가시키는 경우 컨슈머가 `poll()` 메서드를 호출하는 빈도가 낮아져 Kafka 브로커의 heartbeat 지연이 발생할 수 있으므로, 적절한 값으로 설정할 필요가 있다.
2. 컨슈머 인스턴스 추가
    * 컨슈머 인스턴스를 추가하여 파티션 할당을 분산시키는 방법
    * 이러면 각 컨슈머 인스턴스는 더 적은 파티션을 처리하게 되므로, 처리 시간이 긴 작업이 있어도 다른 파티션의 메시지를 처리할 수 있다.
3. 다른 방법은?
    * Kafka 컨슈머 그룹 내에서 파티션 할당이 균등하게 이루어지고, 처리 시간이 긴 작업을 분산시킬 수 있는 다른 방법을 고려한다.
    * 예, 병렬 처리나 스트리밍 처리 등
