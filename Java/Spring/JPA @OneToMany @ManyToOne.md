# JPA @OneToMany @ManyToOne
* JPA에서 관계형 데이터베이스의 테이블 간 일대다(One-to-Many) 및 다대일(Many-to-One) 관계를 표현하는 데 사용
### @OneToMany
* 일대다 관계에서 **일** 쪽 엔티티에 적용되는 어노테이션
* 일대다 관계에서는 한 엔티티가 여러 개의 다른 엔티티와 관계를 가지는 것을 의미한다.
* `@OneToMany` 어노테이션을 사용하면 **엔티티 간의 일대다 관계**를 매핑할 수 있다.
### @ManyToOne
* 다대일 관계에서 **다** 쪽 엔티티에 적용되는 어노테이션
* 다대일 관계에서는 여러 개의 엔티티가 하나의 엔티티와 관계를 가지는 것을 의미한다.
* `@ManyToOne` 어노테이션을 사용하면 **엔티티 간의 다대일 관계**를 매핑할 수 있다.
### 예제 소스
```java
@Entity
public class Post {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    
    @OneToMany(mappedBy="post")
    private List<Comment> comments;
    
    // getter, setter 메서드
}

@Entity
public class Comment {
    @Id
    @GeneratedValue(strategy = GenenrationType.IDENTITY)
    private Long id;
    
    private String content;
    
    @ManyToOne
    @JoinColumn(name = "post_id")
    private Post post;
    
    // getter, setter 메서드
}
```
* `Post` 엔티티와 `Comment` 엔티티는 일대다 관계를 가지고 있다.
* `Post` 엔티티는 여러 개의 `Comment` 엔티티와 관계를 가지며, `Comment` 엔티티는 하나의 `Post` 엔티티와 관계를 가진다.
* `Comment` 엔티티에서는 `@ManyToOne` 어노테이션을 사용하며 `post` 속성을 매핑하고, `@JoinColumn` 어노테이션을 사용하여 `post_id` 컬럼과 매핑한다.
```java
@Service
public class PostService {
    private final PostRepository postRepository;
    private final CommentRepository commentRepository;
    
    public PostService(PostRepository postRepository, CommentRepository commentRepository) {
        this.postRepository = postRepository;
        this.commentRepository = commentRepository;
    }
    
    @Transactional
    public void savePostWithComment() {
        // Create a Post
        Post post = new Post();
        post.setTitle("Sample Post");
        
        Comment comment1 = new Comment();
        comment1.setContent("Comment 1");
        comment1.setPost(post);
        
        Comment comment2 = new Comment();
        comment2.setContent("Comment 2");
        comment2.setPost(post);
        
        // Add comments to the post
        post.getComments().add(comment1);
        post.getComments().add(comment2);
        
        // Save the post (including comment)
        postRepository.save(post);
    }
}
```
* 위 예제 소스는 데이터 저장 예제 소스이다.
* `savePostWithComment()` 메서드는 `Post`와 `Comment` 엔티티를 생성하고, 관계 설정 후 `postRepository.save()`를 호출하여 데이터를 저장한다.
```java
@Service
public class PostService {
    private final PostRepository postRepository;
    private final CommentRepository commentRepository;
    
    public PostService(PostRepository postRepository, CommentRepository commentRepository) {
        this.postRepository = postRepository;
        this.commentRepository = commentRepository;
    }
    
    @Transactional(readOnly = true)
    public List<Post> getAllPosts() {
        return postRepository.findAll();
    }
    
    @Transactional(readOnly = true)
    public Post getPostById(Long postId) {
        return postRepository.findById(postId)
                .orElseThrow(() -> new NotFoundException("Post not found"));
    }
}
```
* 위 예제 소스는 데이터 조회 예제 소스이다.
* `getAllPosts()` 메서드에서는 모든 `Post` 데이터를 조회한다.
* `getPostById()` 메서드에서는 주어진 `postId`를 사용하여 특정 `Post` 데이터를 조회한다.
