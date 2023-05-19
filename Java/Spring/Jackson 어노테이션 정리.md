# Jackson 어노테이션 정리
## Jackson 기본 어노테이션
### @JsonProperty
* **json 직렬화 및 역직렬화를 수행할 때 속성의 이름을 지정**하는 데 사용하는 어노테이션
* 필드 또는 getter, setter 메서드에 사용할 수 있다.
```java
public class User {
    @JsonProperty("username")
    private String name;
}
```
`@JsonProperty("username")`은 해당 필드 또는 getter/setter 메서드가 json 직렬화 및 역직렬화 중에 "username" 속성과 매핑되어야 함을 나타낸다.
### @JsonFormat
* **날짜, 시간 및 숫자**와 같은 특정 타입의 형식을 지정하는 데 사용되는 어노테이션
* 필드 또는 getter, setter 메서드에 사용할 수 있다.
```java
public class Event {
    private String name;
    
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd")
    private LocalDate date;
}
```
* `date` 필드가 json으로 직렬화될 때 `yyyy-MM-dd` 형식으로 지정되어야 함을 나타낸다.
### @JsonUnwrapped
* 객체의 특정 필드를 상위 json 객체에 포함시키는 대신 **해당 필드의 속성을 직접 json으로 펼치는 데** 사용된다.
* 필드에만 사용할 수 있다.
```java
public class Address {
    private String city;
    private String zipcode;
}

public class User {
    private String name;
    
    @JsonUnwrapped
    private Address address;
}
```
* `User` 객체가 json으로 직렬화될 때 `Address` 객체의 속성인 `city`와 `zipcode`가 `User` 객체의 직접적인 속성으로 펼쳐야 함을 나타낸다.
### @JsonView
* **특정 시나리오에서 보여지는** 속성의 그룹을 정의하는 데 사용된다.
* 이를 통해 다른 시나리오에서 동일한 객체라도 다른 속성을 보여줄 수 있다.
```java
public class User {
    @JsonView(Views.Public.class)
    private String name;
    
    @JsonView(Views.Internal.class)
    private String email;
}
```
```java
// View 클래스
public class Views {
    public static class Public {}
    public static class Internal extends Public {}
}
```
* `name` 속성을 `Public` 시나리오에서만 직렬화하고, `email` 속성은 `Internal` 시나리오에서 직렬화함을 나타낸다.
### @JsonIdentityInfo
* 객체 간의 순환 참조 시 json 직렬화 및 역직렬화를 처리하는 데 사용한다.
* 이를 통해 **순환 참조가 발생하는 객체들도 고유한 식별자를 사용**하여 참조 관계를 유지할 수 있다.
```java
@JsonIdentityInfo(generator = ObjectIdGenerators.PropertyGenerator.class, property = "id")
public class User {
    private String id;
    private String name;
    private Group group;
}

public class Group {
    private String id;
    private String name;
    private List<User> users;
}
```
* `id` 속성을 사용하여 `User` 및 `Group` 객체 간의 순환 참조를 처리한다.
* 이를 통해 객체를 **json으로 직렬화할 때 객체 간의 참조 관계가 유지**된다.
### @JsonFilter
* **동적으로 속성 필터링을 수행**하기 위해 제공되는 어노테이션
* **객체 속성을 필터링하여 특정 시나리오에 따라 다른 json 출력**을 생성할 수 있다.
```java
@JsonFilter("userFilter")
public class User {
    private String name;
    private String email;
}
```
* `@JsonFilter` 어노테이션을 사용하기 위해 `ObjectMapper`에 필터를 등록해야 한다.
```java
FilterProvider filters = new SimpleFilterProvider().addFilter("userFilter", SimpleBeanPropertyFilter.filterOutAllExcept("name"));
ObjectMapper mapper = new ObjectMapper().setFilterProvider(filters);

// ...

// name 속성만 포함된 json이 생성됨
String json = mapper.writeValueAsString(user);
```
* 필터를 사용하여 동적으로 속성을 선택적으로 제외하거나 포함시킬 수 있다.
### @JsonManagedReference, @JsonBackReference
* **양방향 참조 관계**를 직렬화할 때 사용되는 어노테이션
* **무한 루프에 빠지지 않고 객체 그래프를 직렬화**할 수 있다.
* **@JsonManagedReference**
  * 양방향 연관 관계에서 **단방향 연관 관계의 속성**에 적용된다.
  * 해당 어노테이션이 붙은 속성은 직렬화할 때 **참조되는 객체를 관리**한다.
* **@JsonBackReference**
  * 양방향 연관 관계에서 **양방향 연관 관계의 속성**에 적용된다.
  * 해당 어노테이션이 붙은 속성은 직렬화할 때 **참조되는 객체를 무시**한다.
```java
public class User {
    private String name;
    
    @JsonManagedReference
    private List<Order> orders;
}

public class Order {
    private String orderId;

    @JsonBackReference
    private User user;
}
```
* `User` 클래스는 `Order` 클래스와 일대다 관계이다.
  * `User` 클래스는 여러 개의 `Order`를 가지고 있고, `Order` 클래스는 하나의 `User`에 속해 있다.
* `@JsonManagedReference ` 는 `User` 클래스의 `orders` 속성에 적용되어 있으며, 이는 직렬화 시 `Order` 목록을 관리하고 직렬화된다.
* `@JsonBackReference ` 는 `Order` 클래스의 `user` 속성에 적용되어 있으며, 이는 직렬화 시 `User` 객체를 무시하고 직렬화된다.
  * 무한 루프에 빠지지 않고 객체 그래프를 직렬화할 수 있다.
* 이후 `User` 객체를 json으로 직렬화하면 `orders` 속성은 포함되지만 각 `Order` 객체에서 다시 `User` 객체로의 참조는 무시된다.
```json
{
  "name": "John Smith",
  "orders": [
    {
      "orderId": "123"
    },
    {
      "orderId": "456"
    }
  ]
}
```
## 직렬화 어노테이션
* 직렬화 : Java 객체를 json 형태로 변환하는 것을 의미
### @JsonGetter
* getter 메서드 위에 사용되는 어노테이션으로, 특정 메서드를 json 직렬화 중에 호출하고 **그 결과를 해당 메서드 이름과 매핑된 필드로 직렬화**한다.
```java
public class User {
    private String name;
    
    @JsonGetter("full_name")
    public String getName() {
        return name;
    }
}
```
* `getName()` 메서드가 "full_name" 필드로 직렬화되어야 함을 나타낸다.
### @JsonAnyGetter
* getter 메서드 위에 사용되는 어노테이션으로, **객체의 동적인 속성을 json 필드로 직렬화**하는 데 사용되는 어노테이션이다.
```java
public class User {
    private Map<String, Object> additionalProperties = new HashMap<>();

    @JsonAnyGetter
    public Map<String, Object> getAdditionalProperties() {
        return additionalProperties;
    }
}
```
* `getAdditionalProperties()` 메서드를 통해 추가적인 속성을 json에 동적으로 직렬화한다.
* `User` 객체의 추가 속성은 json에 해당 속성의 이름과 값으로 포함된다.
### @JsonValue
### @JsonRawValue
### @JsonSerialize
### @JsonPropertyOrder
### @JsonRootName
## 역직렬화 어노테이션
### @JsonCreator
### @JsonSetter
### @JsonAnySetter
### @JsonDeserialize
### @JsonAlias
### @JacksonInject
## Property 포함 어노테이션
### @JsonIgnore
### @JsonIgnoreProperties
### @JsonIgnoreType
### @JsonInclude
### @JsonAutoDetec

