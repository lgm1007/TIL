# Spring Data Redis 두 가지 사용방법
### 1. RedisRepository
* JpaRepository 처럼 인터페이스로 구성
  * `SimpleKeyValueRepository`가 사용됨
  * 조회 등 작업 시 KeyValueOperations의 구현체인 `KeyValueTemplate`가 사용됨
* Redis의 Hash를 이용해 기본적인 CRUD를 수행

```java
@Getter
@RedisHash(value = "member-info")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@AllArgsConstructor
public class Member {
    @Id
    private String id;
    private String name;
    private int age;
}
```
* RedisRepository를 사용하기 위해선 `@RedisHash`를 추가한 클래스가 필요
* `@RedisHash`는 Redis의 key 값의 prefix로 사용할 값을 설정
* `@Id`는 해당 prefix 뒤에 붙으며 `{prefix}:{Id}` 형태로 객체를 판별할 수 있게 해 줌
  * Redis에서 Id 값은 대체로 String 타입

```java
public interface MemberRedisRepository extends CrudRepository<Member, String> {
```
* `CrudRepository`를 상속받은 Repository 인터페이스 생성
* 이후 일반적인 JpaRepository 사용하듯 사용하면 됨


### 2. RedisTemplate
* 각종 Operation 객체를 통해 redis 사용
* 사용하는 자료 구조에 따라 적절한 메서드를 사용

|메서드|설명|
|---|---|
|opsForValue|Object를 쉽게 Serialize / Deserialize 해주는 인터페이스|
|opsForList|List를 쉽게 Serialize / Deserialize 해주는 인터페이스|
|opsForSet|Set을 쉽게 Serialize / Deserialize 해주는 인터페이스|
|opsForZSet|ZSet을 쉽게 Serialize / Deserialize 해주는 인터페이스|
|opsForHash|Hash를 쉽게 Serialize / Deserialize 해주는 인터페이스|

#### CRUD 예제 : String
* opsForValue 메서드로 String 타입의 자료구조에 접근한다.

```java
@Test
public void testString() {
    final String key = "redis_key";
    final ValueOperations<String, String> stringValueOperations = redisTemplate.opsForValue();

    stringValueOperations.set(key, "1");
    final String result1 = stringValueOperations.get(key);

    stringValueOperations.increment(key);
    final String result2 = stringValueOperations.get(key);
    
    assertEquals(result1, "1");
    assertEquals(result2, "2");
}
```

#### CRUD 예제 : List
* opsForList 메서드로 List 타입의 자료구조에 접근한다.

```java
@Test
public void testList() {
    final String key = "redis_key";
    final ListOperations<String, String> stringListOperations = redisTemplate.opsForList();

    stringListOperations.rightPush(key, "H");
    stringListOperations.rightPush(key, "e");
    stringListOperations.rightPush(key, "l");
    stringListOperations.rightPush(key, "l");
    stringListOperations.rightPush(key, "o");

    stringListOperations.rightPushAll(key, " ", "a", "b", "c");
    
    final String character = stringListOperations.index(key, 1);
    final Long size = stringListOperations.size(key);
    final List<String> resultList = stringListOperations.range(key, 0, 8);
    
    assertEquals(character, "e");
    assertEquals(size, 9L);
    assertEquals(resultList, List.of("H", "e", "l", "l", "o", " ", "a", "b", "c"));
}
```

#### CRUD 예제 : Set
* opsForSet 메서드로 Set 타입의 자료구조에 접근한다.

```java
@Test
public void testSet() {
    final String key = "redis_key";
    final SetOperations<String, String> stringSetOperations = redisTemplate.opsForSet();

    stringSetOperations.add(key, "H");
    stringSetOperations.add(key, "e");
    stringSetOperations.add(key, "l");
    stringSetOperations.add(key, "l");
    stringSetOperations.add(key, "o");
    
    Set<String> setElements = stringSetOperations.members(key);
    Long size = stringSetOperations.size(key);
    
    assertEquals(setElement, Set.of("e", "l", "o", "H"));
    assertEquals(size, 4L);
}
```

#### CRUD 예제 : Sorted Set
* opsForZSet 메서드로 Sorted Set 타입의 자료구조에 접근한다.

```java
@Test
public void testZSet() {
    final String key = "redis_key";
    final ZSetOperations<String, String> stringZSetOperations = redisTemplate.opsForZSet();

    stringZSetOperations.add(key, "H", 1);
    stringZSetOperations.add(key, "e", 5);
    stringZSetOperations.add(key, "l", 10);
    stringZSetOperations.add(key, "l", 15);
    stringZSetOperations.add(key, "o", 20);
    
    Set<String> range = stringZSetOperations.range(key, 0, 5);
    Long size = stringZSetOperations.size(key);
    Set<String> scoreRange = stringZSetOperations.rangeByScore(key, 0, 13);
    
    assertEquals(range, Set.of("H", "e", "l", "o"));
    assertEquals(size, 4L);
    assertEquals(scoreRange, Set.of("H", "e")); // Set은 중복 제거가 되기 때문에 "l"은 score = 15
}
```

#### CRUD 예제 : Hash
* opsForHash 메서드로 Hash 타입의 자료구조에 접근한다.

```java
@Test
public void testHash() {
    final String key = "redis_key";
    final HashOperations<String, Object, Object> stringObjectHashOperations = redisTemplate.opsForHash();

    stringObjectHashOperations.put(key, "Hello", "redis_key");
    stringObjectHashOperations.put(key, "Hello2", "redis_key2");
    stringObjectHashOperations.put(key, "Hello3", "redis_key3");

    Object hello = stringObjectHashOperations.get(key, "Hello");
    
    Map<Object, Object> entries = stringObjectHashOperations.entries(key);
    Object hello2 = entries.get("Hello2");
    
    Long size = stringObjectHashOperations.size(key);
    
    assertEquals((String) hello, "redis_key");
    assertEquals((String) hello2, "redis_key2");
    assertEquals(size, 3L);
}
```

