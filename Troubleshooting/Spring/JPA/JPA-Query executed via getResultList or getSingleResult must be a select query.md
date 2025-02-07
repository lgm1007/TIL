# [JPA Error] Query executed via 'getResultList()' or 'getSingleResult()' must be a 'select' query
> org.springframework.dao.InvalidDataAccessApiUsageException: Query executed via 'getResultList()' or 'getSingleResult()' must be a 'select' query

### 문제 발생 상황
* Spring Data JPA 중 `@Query`를 사용하여 JPQL 쿼리 실행 시 잘못된 유형의 쿼리가 사용되었을 때 발생하는 에러이다.
* `getResultList()`나 `getSingleResult()` 는 결과를 반환하는 `select` 쿼리에서만 사용 가능하다. 
* 이를 `insert`, `update`, `delete` 쿼리를 사용하려 하면 위와 같은 에러가 발생한다.

```java
@Query("UPDATE User u SET u.isDeleted = :isDeleted WHERE u.userId = :userId")
void updateUserDeletedById(@Param("userId") Long userId, @Param("isDeleted") boolean isDeleted);
```

### 해결 방법
* 데이터 조작 쿼리를 사용하고자 할 때는 `@Modifying` 어노테이션을 추가로 사용한다.

```java
@Modifying
@Query("UPDATE User u SET u.isDeleted = :isDeleted WHERE u.userId = :userId")
void updateUserDeletedById(@Param("userId") Long userId, @Param("isDeleted") boolean isDeleted);
```
