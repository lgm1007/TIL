# Spring Kafka Consumer
* 스프링 컨슈머는 크게 레코드/배치 타입으로 나누고 7개의 커밋 방식을 구분화한 리스너를 구현하여 사용할 수 있다.
* 스프링 카프카에서는 커밋을 `Ack Mode` 라고 한다.

### Ack Mode 종류
|종류|설명|
|---|---|
|RECORD|레코드 단위로 프로세싱 이후 커밋|
|BATCH|`poll()` 메서드로 호출된 레코드가 모두 처리된 이후 커밋 <br/>스프링 카프카 컨슈머의 Ack Mode 기본값|
|TIME|특정 시간 이후에 커밋 <br/>이 옵션을 사용할 경우 시간 간격을 선언하는 AckTime 옵션을 설정해야 한다.|
|COUNT|특정 개수만큼 레코드가 처리된 이후 커밋 <br/>이 옵션을 사용할 경우에는 레코드 개수를 선언하는 AckCount 옵션을 설정해야 한다.|
|COUNT_TIME|TIME, COUNT 옵션 중 맞는 조건이 하나라도 나올 경우 커밋|
|MANUAL|`Acknowledgement.acknowledge()` 메서드가 호출되면 다음번 `poll()` 떄 커밋을 한다. <br/>매번 `acknowledge()` 메서드를 호출하면 BATCH 옵션과 동일하게 동작한다. <br/>이 옵션을 사용할 경우에는 AcknowledgingMessageListener 또는 BatchAcknowledgingMessageListener를 리스너로 사용해야 한다.|
|MANUAL_IMMEDIATE|`Acknowledgement.acknowledge()` 메서드를 호출한 즉시 커밋한다. <br/>이 옵션을 사용할 경우에는 AcknowledgingMessageListener 또는 BatchAcknowledgingMessageListener를 리스너로 사용해야 한다.|

* 스프링 카프카 컨슈머의 Ack Mode 기본값은 BATCH이고, 컨슈머의 enable.auto.commit 옵션은 false로 지정된다.
