# @TypeDef
### 설명
* Hibernate 에서 사용하는 커스텀 유형 매핑을 정의할 때 사용한다.
  * 기본적으로 Hibernate는 많은 데이터베이스 유형과 자바 데이터 타입 간 매핑을 제공하나, 때때로 사용자 정의 유형을 매핑해야 할 때가 있다.
  * 데이터베이스 열에 저장된 값이 특정 형식이 아닌 **사용자가 정의한 클래스나 열거형**일 경우에는 매핑을 추가로 정의해야 한다.
  * 이런 경우 `@TypeDef` 어노테이션을 사용하여 커스텀 유형의 매핑을 정의할 수 있다.
### @TypeDef 어노테이션 속성
* `name`: 유형 매핑의 이름을 지정한다.
* `typeClass`: 매핑할 유형의 클래스를 지정한다.
* `defaultForType`: 해당 유형 매핑이 기본적으로 사용될 유형을 지정한다.
### 예시 1
```java
@TypeDef(name = "json", typeClass = JsonType.class)
public class Domain {
    // ...
    
    @Type(type = "json")
    @Column(name = "value", columnDefinitions = "longtext")
    private Map<String, Object> value = new HashMap<String, Object>();
}
```
* `JsonType` 클래스를 이용하여 Json 타입으로 매핑
* `value` 라는 컬럼을 해당 json 타입 매핑을 사용하도록 한다.
### 예시 2
```java
@TypeDef(name = "encryptedString", typeClass = EncryptedStringType.class, defaultForType = String.class)
public class User {
    // ...
}
```
* `encryptedString` 이름의 타입 매핑을 정의하고, `EncryptedStringType` 클래스를 사용하여 String 타입을 매핑한다.
* String 타입에 대해서는 이 매핑이 기본적으로 사용될 것이다.
