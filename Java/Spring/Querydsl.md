# Querydsl
### Querydsl 이란?
* Java에서 타입 안전한 쿼리를 작성할 수 있게 도와주는 라이브러리
* 문자열이나 xml 대신 Querydsl이 제공하는 플루언트(Fluent) API를 이용해 쿼리를 생성할 수 있다.
### Querydsl의 특징
* SQL, JPA, MongoDB 등 다양한 데이터베이스를 지원한다.
* 자바 코드로 데이터베이스 쿼리를 작성할 수 있으므로, 오타나 문법 오류를 런타임 이전에 찾아낼 수 있다.
* IDE에서 코드 자동완성과 같은 기능을 사용할 수 있어 개발 효율성이 향상된다.
* 직관적이고 가독성이 좋은 코드를 작성할 수 있다.
    * ORM 프레임워크를 사용하는 경우 코드의 가독성과 유지보수성이 향상된다.
* 동적 쿼리를 작성하는 경우에 유연하게 대처할 수 있어 유용하다.
### Querydsl 예제 소스
```java
JPAQueryFactory queryFactory = new JPAQueryFactory(entityManager);
QUser user = QUser.user;

List<String> names = queryFactory
        .select(user.name)
        .from(user)
        .where(user.age.gt(20))
        .orderBy(user.name.asc())
        .fetch();
```
1. `QUser` Q클래스를 이용하여 `user` 테이블에 대한 정보를 가져온다.
2. `where` 조건절과 `orderBy` 절을 이용하여 쿼리를 작성한다.
3. `fetch` 메서드를 호출하여 결과를 가져온다.

