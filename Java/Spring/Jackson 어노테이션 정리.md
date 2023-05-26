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
* 객체를 json으로 **직렬화할 때 해당 객체의 특정 값을 사용**하도록 하는 어노테이션
* 객체의 특정 필드나 메서드에 해당 어노테이션을 붙여서, 해당 필드나 메서드가 반환하는 값을 json으로 직렬화할 때 사용한다.
* 주로 열거형(Enum) 클래스에서 사용되어 열거형 값을 json으로 직렬화할 때 유용하다.
```java
public enum Size {
    SMALL("S"),
    MEDIUM("M"),
    LARGE("L");
    
    private String abbreviation;
    
    private Size(String abbreviation) {
        this.abbreviation = abbreviation;
    }
    
    @JsonValue
    public String getAbbreviation() {
        return abbreviation;
    }
}
```
* `getAbbreviation()` 메서드에 `@JsonValue` 어노테이션을 추가하여 해당 메서드가 반환하는 값을 직렬화한다.
* `Size.SMALL`은 "S", `Size.MEDIUM`은 "M", `Size.LARGE`는 "L"로 직렬화한다.
### @JsonRawValue
* **json 문자열을 그대로 직렬화**하고자 할 때 사용하는 어노테이션
* 일반적으로 Jackson은 문자열을 json 문자열로 감싸서 직렬화하지만, `@JsonRawValue` 어노테이션을 사용하면 **문자열을 그대로 json으로 직렬화**한다.
```java
public class RawValueExample {
    private String jsonString;
    
    @JsonRawValue
    public String getJsonString() {
        return jsonString;
    }
    
    public void setJsonString(String jsonString) {
        this.jsonString = jsonString;
    }
}
```
* `getJsonString()` 메서드에 `@JsonRawValue` 어노테이션을 추가하여 해당 메서드가 반환하는 json 문자열을 그대로 직렬화한다.
* 즉 `{"key": "value"}`와 같은 문자열이 `{"key": "value"}`로 그대로 직렬화된다.
### @JsonSerialize
* 특정 필드나 메서드에 대해 **직렬화를 커스터마이징하기 위해 사용**하는 어노테이션
* 해당 필드나 메서드의 값을 반환하거나, 사용자 정의 직렬화 로직을 적용할 수 있다.
```java
@JsonSerialize(using = CustomSerializer.class)
public class CustomSerializeExample {
    // 필드와 메서드...
}
```
```java
public class CustomSerializer extends JsonSerializer<CustomSerializeExample> {
    
    @Override
    public void serialize(CustomSerializerExample example, JsonGenerator jsonGenerator, SerializerProvider serializerProvider) throws IOException {
        // 원하는 방식으로 필드 값을 변환하거나 로직을 적용하여 직렬화 로직을 작성한다.
        // 예제 소스에서는 name 필드를 대문자로 변환하여 직렬화하는 로직을 작성했다.
        String upperCaseName = example.getName().toUpperCase();
        
        // json 생성기를 사용하여 필드를 직렬화한다.
        jsonGenerator.writeStartObject();
        jsonGenerator.writeStringField("name", upperCaseName);
        jsonGenerator.writeEndObject();
    }
}
```
* `CustomSerializeExample` 클래스는 `CustomSerializer`를 사용하여 직렬화를 수행한다.
  * `CustomSerializer`는 `JsonSerializer` 클래스를 상속받은 사용자 정의 직렬화 클래스로, `CustomSerializeExample` 객체를 직렬화할 때 사용자가 원하는 방식의 값을 반환하거나 로직을 적용할 수 있다.
### @JsonPropertyOrder
* 객체를 직렬화할 때 json 속성의 순서를 지정하는데 사용하는 어노테이션
* 속성의 순서를 지정하여 json 출력을 예측 가능하게 만들거나, 특정 규칙에 따라 속성을 정렬할 수 있다.
```java
@JsonPropertyOrder({ "name", "age", "email" })
public class Person {
    private String name;
    private int age;
    private String email;
    
    // getter, setter 메서드
}
```
* `@JsonPropertyOrder` 어노테이션으로 출력될 json 속성의 순서를 `name`, `age`, `email` 순으로 지정한다.
### @JsonRootName
* json 직렬화 시 최상위 루트 객체의 이름을 지정하는 데 사용하는 어노테이션
* 일반적으로 Jackson은 최상위 루트 객체의 이름을 클래스 이름으로 사용하지만, 해당 어노테이션을 사용하여 다른 이름으로 지정 가능하다.
```java
@JsonRootName("customer")
public class User {
    private String name;
    private int age;

  // getter, setter 메서드
}
```
* `@JsonRootName` 어노테이션으로 json 직렬화 시 최상위 루트 객체의 이름을 `customer`로 지정한다.
  * json 출력 시 최상위 루트 객체의 이름이 "customer" 로 표시된다.
## 역직렬화 어노테이션
### @JsonCreator
* json 데이터를 **역직렬화하여 객체를 생성할 때 사용되는 생성자나 정적 팩토리 메서드를 지정**하는 데 사용된다.
* 기본적으로 기본 생성자와 setter 메서드를 사용하여 객체를 생성하고 값을 설정하지만, `@JsonCreator` 어노테이션을 사용하면 **직접 생성자나 정적 팩토리 메서드를 지정**하여 객체를 생성할 수 있다.
```java
public class Person {
    private String name;
    private int age;
    
    @JsonCreator
    public Person(@JsonProperty("name") String name, @JsonProperty("age") int age) {
        this.name = name;
        this.age = age;
    }
    
    // getter, setter 메서드
}
```
* 위 예제는 생성자에 사용한 예제이다.
```java
public class Person {
    private String name;
    private int age;
    
    private Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @JsonCreator
    public static Person createPerson(@JsonProperty("name") String name, @JsonProperty("age") int age) {
        return new Person(name, age);
    }
    
    // getter, setter 메서드
}
```
* 위 예제는 정적 팩토리 메서드에 사용한 예제이다.
### @JsonSetter
* json 데이터를 역직렬화하여 **객체의 필드에 값을 설정**할 때 사용한다.
* 기본적으로 setter 메서드를 사용하여 객체의 필드에 값을 설정하지만, `@JsonSetter` 어노테이션을 사용하면 **setter 메서드를 직접 지정**하여 객체의 필드에 값을 설정할 수 있다.
```java
public class Person {
    private String name;
    private int age;
    
    @JsonSetter("personName")
    public void setName(String name) {
        this.name = name;
    }
    
    @JsonSetter("personAge")
    public void setAge(int age) {
        this.age = age;
    }
    
    // getter ...
}
```
* `personName` 필드는 `setName()` 메서드를 통해 `name` 필드에 값을 설정하고, `personAge` 필드는 `setAge()` 메서드를 통해 `age` 필드에 값을 설정한다.
### @JsonAnySetter
* json 역직렬화 시 **알려지지 않은 필드를 동적으로 처리하기 위해** 사용되는 어노테이션
* 일반적으로는 알려진 필드에 대해서는 매핑이 자동으로 이뤄지지만, 알려지지 않은 필드에 대해서는 자동으로 매핑이 이뤄지지 않는다.
  * 이럴 때 동적으로 필드를 처리하기 위해 사용하는 어노테이션
```java
public class CustomObject {
    private String name;
    private Map<String, Object> additionProperties = new HashMap<>();
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    @JsonAnySetter
    public void setAdditionProperties(String key, Object value) {
        additionProperties.put(key, value);
    }
    
    public Map<String, Object> getAdditionProperties() {
        return additionProperties;
    }
}
```
* `@JsonAnySetter` 어노테이션을 사용하여 `setAdditionalProperties()` 메서드를 정의
* `CustomObject` 객체로 역직렬화할 때 알려지지 않은 필드가 있으면 해당 필드는 `setAdditionalProperties()` 메서드를 통해 동적으로 처리된다.
### @JsonDeserialize
* json 역직렬화 시 특정한 커스텀 역직렬화 클래스를 사용하도록 지정하는 데 사용하는 어노테이션
* 기본적으로 Jackson은 자동으로 역직렬화를 수행하지만, 특정 필드나 클래스에 대해 커스텀한 역직렬화 로직을 적용하고자 할 때 `@JsonDeserialize` 어노테이션을 사용할 수 있다.
```java
@JsonDeserialize(using = CustomDeserializer.class)
public class MyCustomClass {
    // fields, constructors, methods
}
```
* `@JsonDeserialize` 어노테이션을 사용해 `MyCustomClass` 클래스에 `CustomDeserializer` 클래스를 사용하여 역직렬화하도록 지정한다.
* Jackson은 `CustomDeserializer` 클래스의 역직렬화 로직을 사용하여 json 데이터를 `MyCustomClass` 객체로 변환한다.
### @JsonAlias
* json 필드의 다양한 이름에 대한 매핑을 설정하는 데 사용하는 어노테이션
* json 데이터에 동일한 값을 가진 여러 필드가 있을 때, `@JsonAlias` 어노테이션을 사용하여 해당 필드들을 하나의 Java 필드와 매핑할 수 있다.
```java
public class MyData {
    @JsonAlias({ "name", "fullName" })
    private String username;
    
    // getters, setters ...
}
```
* `@JsonAlias` 어노테이션을 사용하여 `username` 필드를 `name`과 `fullName` 두 개의 json 필드와 매핑하고 있다.
  * json 데이터에서 `name`이나 `fullName` 필드가 있는 경우 해당 값은 `username` 필드에 매핑된다.
### @JacksonInject
* 주입할 값을 지정하는 데 사용하는 어노테이션
* 주로 Jackson이 json 데이터를 역직렬화하여 Java 객체로 변환할 때 외부에서 주입된 값을 사용하고자 할 때 사용한다.
```java
public class MyData {
    private String name;
    
    @JacksonInject("defaultName")
    private String defaultName;
    
    // getter, setters ...
}
```
* `@JacksonInject` 어노테이션을 사용하여 `defaultName` 필드에 `defaultName` 이라는 이름으로 주입된 값을 사용하도록 지정한다.
  * 이러면 Jackson은 `defaultName` 필드에 주입된 값을 사용하여 역직렬화한 Java 객체를 생성한다.
## Property 포함 어노테이션
### @JsonIgnore
### @JsonIgnoreProperties
### @JsonIgnoreType
### @JsonInclude
### @JsonAutoDetec

