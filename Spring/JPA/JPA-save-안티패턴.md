# JPA `save()` 안티패턴
### `JpaRepository.save()` 가 안티패턴인 경우?
* `JpaRepository`는 `CrudRepository`로부터 save 메서드를 상속받는다.
* `JpaRepository.save()` 내부 구현

```java
@Transactional
public <S extends T> S save(S entity) {
    if (this.entityInformation.isNew(entity)) {
        this.em.persist(entity);
        return entity;
    } else {
        return this.em.merge(entity);
    }
}
```

* `JpaRepository`가 save 메서드를 제공하기 때문에, 대부분 아래와 같이 안티패턴에 빠지게 된다.

```java
@Transactional
public void savePostAntipattern(Long postId, String postTitle) {
    Post post = postRepository.findById(postId).orElseThrow();
    post.setTitle(postTitle);
    postRepository.save(post);
}
```

* 위 코드에서 `save()` 부분은 불필요하다.
* save 메서드가 새로운 객체인지 판단하지 못할 때도 있다. 
  * 만약 식별자가 할당된 엔티티라면 JPA는 `persist` 대신 `merge`를 호출하게 된다.
  * 그렇게 되면 불필요한 조회 쿼리가 실행될 수 있다.
  * 이는 배치 프로세싱 작업에서 문제가 될 수 있다.

### `JpaRepository.save()`의 대안
* **커스텀 레포지토리 인터페이스**를 만든다.
* 해당 인터페이스는 JpaRepository의 save를 `deprecate` 시킨다.
* 해당 인터페이스는 JPA의 스펙에 맞게 `persist`, `merge`, `update`와 같은 메서드를 사용하도록 강제한다.

```java
public interface HibernateCustomRepository<T> {
  @Deprecated
  <S extends T> S save(S entity);

  @Deprecated
  <S extends T> S saveAndFlush(S entity);

  @Deprecated
  <S extends T> List<S> saveAll(Iterable<S> entities);

  @Deprecated
  <S extends T> S saveAllAndFlush(Iterable<S> entities);


  <S extends T> S persist(S entity);

  <S extends T> S persistAndFlush(S entity);

  <S extends T> List<S> persistAll(Iterable<S> entities);

  <S extends T> List<S> persist(Iterable<S> entities);


  <S extends T> S merge(S entitiy);

  <S extends T> S mergeAndFlush(S entitiy);

  <S extends T> List<S> mergeAll(Iterable<S> entitiies);

  <S extends T> List<S> mergeAllAndFlush(Iterable<S> entitiies);


  <S extends T> S update(S entity);

  <S extends T> S updateAndFlush(S entity);

  <S extends T> List<S> updateAll(Iterable<S> entities);

  <S extends T> List<S> updateAllAndFlush(Iterable<S> entities);
}
```

* 구현은 위와 같이 하고 사용은 아래처럼 사용한다.

```java
@Repository
public interface PostRepository extends HibernateCustomRepository<Post>, JpaRepository<Post, Long> {}
```
