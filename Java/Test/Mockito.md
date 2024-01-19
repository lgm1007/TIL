# Mockito
* Java의 오픈소스 테스트 프레임워크
* Mockito를 사용하면 실제 객체를 모방한 가짜 객체, Mock 객체 생성이 가능해진다.
* 언제 가짜 객체, Mock 객체가 필요한가?
  * 외부 서비스의 API client 또는 DB와의 연결이 포함된 비즈니스 로직과 같이 복잡한 연관성을 가진 프로그램을 테스트할 때
* **예시**
```java
@Service
public class MemberService {
    public Member getMember() {
        // DB에서 조회해야 하는 작업으로 부하가 걸리는 작업
    }
}
```
* 위 `getMember()` 메서드처럼 DB를 읽어오는 작업을 매 테스트마다 수행하게 되면 부하가 크고 시간도 오래 걸린다.
* Member 정보를 DB에서 조회하지 않고 가짜 객체, Mock으로 만들어 DB 접근을 최소화할 수 있도록 할 수 있다.

### Mockito를 사용한 테스트코드
* 가짜 객체를 만들어 사용하고 싶은 클래스에 `@Mock` 어노테이션 또는 `@MockBean` 어노테이션을 사용해 필드 주입한다.

#### `@Mock` 기본 예제
* `@Mock`으로 만든 가짜 객체의 메서드를 사용하려면 반드시 스터빙(stubbing)을 해야 한다.
  * Mockito에서는 `when` 메서드를 이용해서 스터빙을 지원한다.
```java
@Service
public class MemberService {
    public Member getMember() {
        return new Member("1234", "john");
    }
    
    public int getLoginNumber() {
        return 1;
    }
}
```
```java
public class MockitoTest {
    @Mock
    MemberService memberService;
    
    @Test
    void testReferenceType() {
        assertNull(memberService.getMember());   // Test Pass
    }
    
    @Test
    void testPrimitiveType() {
        assertEquals(0, memberService.getLoginNumber());    // Test Pass
    }
}
```
* `@Mock`을 사용하고 스터빙을 하지 않으면 referenceType은 null, primitiveType은 0을 반환한다.

#### `@Spy` 기본 예제
* `@Spy`로 만든 객체는 진짜 객체이며 메서드 실행 시 스터빙을 하지 않으면 기존 객체의 로직을 실행한 값, 스터빙을 한 경우엔 스터빙 값을 반환한다.
```java
@Service
public class MemberService {
    public Member getMember() {
        return new Member("1234", "john");
    }
}
```
```java
public class MockitoTest {
    @Spy
    MemberService memberService;
    
    @Test
    void testSpyNoStubbing() {
        Member member = memberService.getMember();
        assertEquals("1234", member.getId());     // Test Pass
    }
    
    @Test
    void testSpyWithStubbing() {
        Member memberDummy = new Member("5678", "brown");
        when(memberService.getMember()).thenReturn(memberDummy);    // stubbing O
        Member member = memberService.getMember();
        assertEquals(memberDummy.getId(), member.getId());      // Test Pass
    }
}
```

#### `@InjectMocks`
* `@InjectMocks`는 DI를 `@Mock`이나 `@Spy`로 생성된 mock 객체를 자동으로 주입해주는 어노테이션이다.
```java
public class BoardService {
    MemberService memberService;
    CommentService commentService;
    
    BoardService(MemberService memberService) {
        this.memberService = memberService;
    }
    
    public Member getMember() {
        return memberService.getMember();
    }
    
    public Comment getComment() {
        return commentService.getComment();
    }
}
```
* 테스트를 위해 MemberService와 CommentService를 주입받아 사용하는 BoardService 클래스를 만든다.

```java
public class MockitoTest {
    @Mock
    MemberService memberService;
    
    @Spy
    CommentService commentService;
    
    @InjectMocks
    BoardService boardService;
    
    @Test
    void testGetMember() {
        assertNull(boardService.getMember());      // Test Pass
    }
    
    @Test
    void testGetComment() {
        Comment comment = commentService.getComment();  // Comment("1", "1234", "댓글");
        assertEquals("1", comment.getId());     // Test Pass
    } 
}
```
* `@InjectMocks`를 사용하여 `@Mock`, `@Spy`로 만든 객체들이 자동으로 주입된 것을 확인할 수 있다.
