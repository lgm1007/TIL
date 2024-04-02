# TransientPropertyValueException: save the transient instance before flushing
### 에러 로그
```
org.hibernate.TransientPropertyValueException: object references an unsaved transient instance - save the transient instance before flushing
```

### 에러 원인
* JPA 연관 관계에서 FK로 사용되는 컬럼값이 없는 상태에서 데이터를 저장하려고 시도하다 발생한 에러
* 예시
  * `Student (id, name)` 이라는 테이블과 `School (id, school_name, student_id)` 이라는 테이블이 있을 때, `Student` 테이블에 데이터를 넣지 않고 `School` 테이블에 데이터를 먼저 넣으려고 하면 `student_id` 값을 가진 `Student` 데이터가 없어서 에러가 발생한다.

### 해결 방법
1. FK를 ID로 가지는 엔티티를 우선 저장한다.
2. 연관 관계 매핑할 때 `@OneToOne`, `@ManyToOne`, `@OneToMany` 어노테이션에 `cascade` 옵션을 설정한다.
    * 영속성 전이: 특정 엔티티를 영속화할 때 연관된 엔티티도 함께 영속화하는 것
    * 저장할 때만 사용: `cascade = CascadeType.PERSIST`, 전체 적용: `cascade = CascadeType.ALL`
