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
### Q-Type
* Querydsl 에서는 엔티티로 설정된 클래스에 Q 모델이라는 쿼리 타입 클래스를 미리 생성해놓고 메타데이터로 사용하여 쿼리를 메서드 기반으로 작성한다.
* 엔티티들은 `compileQuerydsl` 을 이용해 Q-Type 으로 변환된다.
* Q-Type 객체 사용하는 방법
```java
// 1) alias 별도 지정 방법
QUser user = new QUser("u");

// 2) 기본 인스턴스 사용
QUser user = QUser.user;
```
* Q-Type 클래스를 생성하려면 우선 프로젝트 설정에 Querydsl 설정을 추가해줘야 한다.
  * gradle 사용 예시
```gradle
plugins {
  // ...
  // querydsl 추가
  id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
}

dependencies {
  // ...
  implementation "com.querydsl:querydsl-jpa:5.0.0"
  implementation "com.querydsl:querydsl-apt:5.0.0"
}

def querydslDir = "$buildDir/generated/querydsl"

querydsl {
    jpa = true
    querydslSourcesDir = querydslDir
}

sourceSets {
    main.java.srcDir querydslDir
}

compileQuerydsl {
    options.annotationProcessorPath = configurations.querydsl
}

configurations {
    // ...
    querydsl {
        extendsFrom compileClasspath
    }
}

// ...
```
* gradle 설정을 추가하면 IDE 빌드 작업목록에 `compileQuerydsl` 이 추가된다.
  * `./gradlew compileQuerydsl` 또는 `./gradlew build` 수행 시 Q-Type 클래스가 생성된다.
    * 위 설정대로라면 `$buildDir/generated/querydsl` 에 생성된다.
### Querydsl 기본 문법
1. 검색 기능
```java
member.username.eq("member1")   // username == "member1"
member.username.ne("member1")   // username != "member1"
member.username.eq("member1").not() // username != "member1"

member.username.isNotNull()

member.age.in(10, 20)   // age in (10, 20)
member.age.notIn(10, 20)   // age not in (10, 20)

member.age.between(10, 30)  // age >= 10 && age <= 30
member.age.goe(30)  // age >= 30
member.age.gt(30)   // age > 30
member.age.loe(30)  // age <= 30
member.age.lt(30)   // age < 30

member.username.like("member%") // like "member%" 검색
member.username.contains("member")  // like "%member%" 검색
member.username.startsWith("member")    // like "member%" 검색
```
2. 결과 조회
  * `fetch()`: 리스트 조회, 데이터 없으면 빈 리스트 반환
  * `fetchOne()`: 단 건 조회
    * 결과가 없으면 null
    * 결과가 둘 이상이면 com.querydsl.core.NonUniqueResultException
  * `fetchFirst()`: `limit(1).fetchOne()`
  * `fetchResults()`: 페이징 쿼리를 같이 수행하여, total count 쿼리 추가
  * `fetchCount()`: count 쿼리, count 수만 조회
```java
List<Member> fetch = queryFactory
        .selectFrom(member)
        .fetch();

Member fetchOne = queryFactory
        .selectFrom(member)
        .fetchOne();

Member.oneMember = queryFactory
        .selectFrom(member)
        .fetchFirst();

QueryResults<Member> results = queryFactory
        .selectFrom(QMember.member)
        .fetchResults();

List<Member> content = results.getResults();

long total = queryFactory.selectFrom(member)
        .fetchCount();
```
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
* `QUser` Q클래스를 이용하여 `user` 테이블에 대한 정보를 가져온다.
* `where` 조건절과 `orderBy` 절을 이용하여 쿼리를 작성한다.
* `fetch` 메서드를 호출하여 결과를 가져온다.

