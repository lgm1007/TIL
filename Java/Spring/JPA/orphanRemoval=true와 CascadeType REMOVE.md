# orphanRemoval = true와 CascadeType.REMOVE 의 차이
```java
@Entity
public class MemberGroup {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String groupName;
    
    @OneToMany(
            mappedBy = "memberGroup",
            fetch = FetchType.LAZY,
            cascade = CascadeType.REMOVE,
            orphanRemoval = true
    )
    private List<Member> members = new ArrayList<>();
}

@Entity
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn
    private MemberGroup memberGroup;
}
```
### CascadeType.REMOVE
* `CascadeType.REMOVE`는 부모 엔티티가 삭제되면 자식 엔티티도 삭제된다. 즉 부모가 자식의 생명 주기를 관리한다.
* 해당 옵션은 **부모와 자식의 관계를 제거해도 자식 엔티티는 삭제되지 않고 그대로 남아있다.**
### orphanRemoval = true
* `orphanRemoval = true`도 마찬가지로 부모 엔티티가 삭제되면 자식 엔티티도 삭제된다.
* 해당 옵션은 **부모와 자식 관계를 제거하면 자식은 고아 객체가 되고, 고아 객체는 제거된다.**
