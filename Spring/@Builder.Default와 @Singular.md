# lombok의 @Builder 기능 활용하기
## @Builder.Default와 @Singular
### @Builder.Default
* lombok의 `@Builder`를 이용해서 객체를 생성할 때 값을 설정하지 않은 필드의 경우 해당 타입의 초기값으로 들어가게 된다.
* `@Builder.Default`는 직접 필드의 기본값을 설정해주고 싶은 경우 사용할 수 있다.
  * 특히 lombok의 `@Builder`로 객체 생성 시 List와 같은 컬렉션 타입 필드를 설정해주지 않고 만들 경우 빈 컬렉션이 아닌 `null`로 초기화된다.
  * `@Builder`로 객체를 만들어줄 때 Object는 제네릭 타입으로 지정하여 초기화해주기 때문
* 예시
  * ```java
    @Getter
    @Setter
    @ToString
    @Builder
    public class GroupDto {
        public int groupId;
        @Builder.Default
        public String groupName = "A 그룹";
        @Builder.Default
        public List<String> groupMembers = new ArrayList<>();
    }
    ```

### @Singular
* `@Singular` 어노테이션은 컬렉션 필드에 대해서 단일 요소를 추가하는 메서드를 작성하기 위해 사용한다.
* 예시
  * ```java
    // @Builder 사용한 Dto 정의
    @Getter
    @Setter
    @ToString
    @Builder
    public class GroupDto {
        public int groupId;
        @Builder.Default
        public String groupName = "A 그룹";
        @Singular("groupMemberItem")
        public List<String> groupMembers;
    }
    
    // 정의한 Dto 객체 사용하는 부분
    GroupDto groupDto = GroupDto.builder()
        .groupId(2)
        .groupName("B 그룹")
        .groupMemberItem("Paul")
        .groupMemberItem("James")
        .groupMemberItem("Brown")
        .build();
    ```
