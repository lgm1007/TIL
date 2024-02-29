# No serializer found for class
### 에러 상황
* 스프링 Jackson 라이브러리가 Serialize하는 과정에 접근 제한자가 `public`이거나 `Getter/Setter`를 이용을 하는데, **필드가 `private`로 선언되어 있거나 `Getter/Setter`가 없는 경우** Json 변환 과정에서 문제가 발생한다.

### 해결 방안
##### 첫 번째 방안
* `ObjectMapper`의 인스턴스에 설정으로 `private` 필드로 접근 가능하도록 한다.
```java
ObjectMapper mapper = new ObjectMapper();
mapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY);
```
* 위 예제 코드처럼 `FIELD`로 접근하고, 접근 제한자 범위를 `ANY`로 설정해준다.

##### 두 번째 방안
* `@JsonProperty` 또는 `@JsonAutoDetect` 어노테이션을 이용하여 해결할 수 있다.
  * [어노테이션 관련 설명 문서](../../Java/Spring/Jackson%20어노테이션%20정리.md)
* 해당 방법으로 해결할 때 Json으로 변환을 원치 않는 필드에는 `@JsonIgnore`을 선언해줘야 한다.
* `@JsonProperty` 사용 예제
```java
public class Member {
    @JsonProperty
    private String name;
    @JsonProperty
    private String email;
    @JsonProperty("addr")
    private String address;
    
    public Member() {
        // ...
    }
}
```
* `@JsonAutoDetect` 사용 예제
```java
// ANY로 지정하면 private 필드에도 접근하여 Json 변환 가능
@JsonAutoDetect(fieldVisibility = JsonAutoDetect.Visibility.ANY)
public class Member {
    private String name;
    private String email;
    private String address;
    
    public Member() {
        // ...
    }
}
```

