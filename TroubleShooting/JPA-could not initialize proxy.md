# [JPA Error] could not initialize proxy - no Session
* `could not initialize proxy - no Session` 에러는 JPA 에서 지연 로딩 (Lazy Loading)을 사용하는 경우 발생할 수 있는 에러이다.
* 이 에러는 프록시 객체가 로딩되려고 할 때 영속성 컨텍스트 (Session)가 없는 상태에서 발생하는 문제이다.
  * 지연 로딩은 연관된 엔티티가 실제로 필요한 시점에 데이터베이스에서 가져오는 것을 의미한다.
  * 이 때 영속성 컨텍스트가 필요한데, 영속성 컨텍스트가 없는 상태에서 지연 로딩을 시도하면 해당 에러가 발생한다.

### 문제 해결
* 이 에러를 해결하기 위해서는 지연 로딩이 발생하는 코드 블록에서 영속성 컨텍스트가 활성화되어야 한다.
1. **Eager Loading 으로 변경하기**
    * 연관된 엔티티를 지연 로딩이 아닌 즉시 로딩 (Eager Loading)으로 변경하는 가장 간단한 해결 방법
    * 이렇게 변경하면 프록시 객체가 아닌 실제 엔티티가 로딩된다.
    * 이는 성능과 관련된 고려사항일 수 있어 신중하게 선택해야 한다.
2. **영속성 컨텍스트 활성화하기**
    * 지연 로딩을 유지한 채로 영속성 컨텍스트를 활성화하는 방법이다.
    * 이는 **트랜잭션 내에서 엔티티를 조회하는 코드 블록에 `@Transactional` 어노테이션을 사용**하여 영속성 컨텍스트를 활성화하는 것이다.
```java
public class ParentService {
	
	private final ParentRepository parentRepository;
	
	@Transactional
    public Parent getParentWithChildren(Long parentId) {
		Parent parent = parentRepository.findById(parentId).orElse(null);
		
		if (parent != null) {
			// 해당 메서드 내에서 프록시 객체 로딩이 필요한 경우 영속성 컨텍스트가 활성화됨
            List<Child> children = parent.getChildren();
		}
		return parent;
    }
}
```
* 위 코드에서 `@Transactional` 어노테이션은 영속성 컨텍스트를 활성화하며, **메서드 내에서 연관된 엔티티를 로딩할 때 프록시 객체가 아닌 실제 엔티티가 로딩**된다.
* 이를 통해 `could not initialize proxy - no Session` 에러를 해결할 수 있다.
