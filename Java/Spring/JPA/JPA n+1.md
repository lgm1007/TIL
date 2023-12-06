# JPA N+1 문제
### N+1 문제
* 연관 관계가 설정된 엔티티를 조회할 경우 조회된 **데이터 갯수(n) 만큼 연관관계의 조회 쿼리가 추가로 발생**하여 데이터를 읽어오는 현상
  * 엔티티 조회 쿼리(1 번) + 조회된 엔티티 개수(n 개) 만큼 연관된 엔티티를 조회하기 위한 추가 쿼리(n 번)
### 발생하는 상황
```java
@Entity
@Getter
@Setter
public class Post {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String content;
    
    @OneToMany(fetch = FetchType.EAGER)
    private List<Comment> comments = new ArrayList<>();
    
    public Comment writeComment(final String content) {
        Comment comment = new Comment(content, this);
        this.comments.add(comment);
        return comment;
    }
}
```
```java
@Entity
@Getter
@Setter
public class Comment {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String content;
    
    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "post_id")
    private Post post;
}
```
* 예제 상황: 게시글과 댓글 엔티티
  * 하나의 게시글에는 여러 개의 댓글이 달릴 수 있다.
* `FetchType`을 `EAGER` (**즉시 로딩**)으로 설정한 경우
  * `postRepository.findAll()`을 호출한 경우 → N + 1 **문제 발생**
* `FetchType`을 `LAZY` (**지연 로딩**)으로 설정한 경우
  * `postRepository.findAll()`을 호출한 경우 → N + 1 문제 **발생하지 않음**
  * `postRepository.findAll()`로 조회한 리스트에서 comment를 사용하는 경우 → N + 1 **문제 발생**
    * ```java
      List<Comment> all = postRepository.findAll();
      all.stream().forEach(comment -> {
          comment.getContent();
      });
      ```
* **발생하는 시점**만 다를 뿐 즉시 로딩이나 지연 로딩 모두 N + 1 문제가 발생한다.
### 문제 발생 이유
* JPA가 JPQL을 분석해서 SQL을 생성할 떄 글로벌 Fetch 전략을 참고하지 않고 오직 JPQL 자체만 사용하기 때문
1. **Fetch 전략이 즉시 로딩인 경우**
   1. `findAll()` 시 `select p from Post p` JPQL 구문이 생성되고, 이를 분석하여 `select * from Post` 라는 SQL 구문이 실행된다.
   2. DB의 결과를 받아 `Post` 엔티티의 인스턴스를 생성한다.
   3. `Post`외 연관되어 있는 `Comment` 도 로딩하기 위해 영속성 컨텍스트에서 연관된 `Comment` 가 있는지 확인한다.
   4. 영속성 컨텍스트에 없다면 생성된 `Post` 인스턴스들의 개수에 맞게 `select * from Comment where post_id = ?`라는 SQL 구문이 실행된다. (N + 1 발생)
2. **Fetch 전략이 지연 로딩인 경우**
   1. `findAll()` 시 `select p from Post p` JPQL 구문이 생성되고, 이를 분석하여 `select * from Post` 라는 SQL 구문이 실행된다.
   2. DB의 결과를 받아 `Post` 엔티티의 인스턴스를 생성한다.
   3. 코드 중에서 `Post`의 `Comment` 객체를 사용하려 하는 시점에 영속성 컨텍스트에서 연관된 `Comment`가 있는지 확인한다.
   4. 영속성 컨텍스트에 없다면 생성된 `Post` 인스턴스들의 개수에 맞게 `select * from Comment where post_id = ?`라는 SQL 구문이 실행된다. (N + 1 발생)
### 해결 방법
1. **Fetch Join**
    * JPQL을 사용하여 DB에서 데이터를 가져올 때 **처음부터 연관된 데이터까지 같이 가져오도록** 하는 방법
    * 별도의 메서드를 만들어주며 `@Query` 어노테이션을 사용해 `join fetch 엔티티.연관관계_엔티티` 구문을 만들어 준다.
    * ```java
      public interface PostRepository extends JpaRepository<Post, Long> {
          @Query("select p from Post p join fetch p.comments")
          List<Comment> findAllFetchJoin();
      }
      ```
2. **EntityGraph 어노테이션**
    * `@EntityGraph` 라는 어노테이션을 사용해서 fetch join을 하는 방법
    * 관계가 복잡해지는 문제가 있어 러닝 커브가 꽤 크다.
3. **Batch Size**
    * N + 1 문제가 발생하더라도 `select * from Comment where post_id = ?` 이 아닌 `select * from Comment where post_id in (?, ?, ?)` 방식으로 N + 1 문제가 발생하게 하는 방법
    * Post 엔티티 갯수가 100개라면 100번 일어나게 될 조회를 1번만 조회하도록 하는 방식으로 성능을 최적화할 수 있는 방법
