# JPA 벌크 업데이트 처리하기
## 부제: @Modifying 어노테이션 사용하기

### 개요
* 만약 JPA에서 수 많은 벌크성 데이터를 업데이트해야 하는 경우 어떻게 할 수 있을까?
* 일반적인 JPA의 변경 감지에 의한 업데이트를 벌크성으로 수행하게 되면 성능에 영향을 줄 수밖에 없다.
* JPA에서 벌크성 데이터 연산 처리를 하는 방법에 대해 알아보도록 하자.

### @Modifying
* `@Query`로 벌크로 수행하려는 쿼리를 작성 후 `@Modifying` 어노테이션을 추가해준다.
```java
@Modifying(clearAutomatically = true, flushAutomatically = true)
@Query("update Member m set m.age = m.age + 1 where m.birthYear >= :birthYear")
int bulkMemberAgeUp(@Param("birthYear") int birthYear);
```

* `@Modifying` 어노테이션은 `@Query`로 작성한 Insert, Update, Delete 구문을 사용할 때 사용해주며, 보통 벌크 연산을 하나의 쿼리로 수행할 때 사용한다.

### 주의사항
* `@Modifying`을 사용하는 메서드는 반드시 반환 타입을 void, int, Integer로 지정해줘야 한다. 그 외의 타입은 오류 발생
* `@Modifying`를 사용하게 되면 영속성 컨텍스트를 무시하고 바로 DB에 접근하여 쿼리를 수행하기 때문에 영속성 컨텍스트와 DB 간의 데이터 싱크가 맞지 않을 수 있다.
  * 사실 `@Modifying`을 사용한 연산 뿐 아니라 순수 JPA, JPQL, QueryDSL을 사용한 벌크 연산 모두 위와 같은 **영속성 컨텍스트 데이터 싱크 문제**가 존재한다.
  * 이를 해결하기 위해 `@Modifying` 에는 `clearAutomatically`와 `flushAutomatically` 옵션이 존재한다.
  * `clearAutomatically`: true로 설정하게 되면, 쿼리 실행 후 영속성 컨텍스트의 내용을 초기화한다.
  * `flushAutomatically`: true로 설정하게 되면, 쿼리 실행 전 영속성 컨텍스트의 변경 사항을 DB에 flush한다.
