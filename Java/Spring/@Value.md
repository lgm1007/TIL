# @Value
### Property 값 주입
* Spring boot 프로젝트가 커지면서 공통으로 사용되는 글로벌 값을 별도로 관리할 필요가 발생
* `@Value` 어노테이션
	* properties (또는 yaml) 파일애 새팅한 내용을 변수에 주입하는 역할 수행
### @Value 사용
1. 일반적인 입력값 주입
```java
public class TestClass {
	@Value("Hello World!")
	private String helloWorld;
	
	@Value("1.234")
	private double doubleValue;
	
	@Value("123")
	private Integer integerValue;
	
	@Value("true")
	private Booleans booleanValue;
	
	@Value("20L")
	private long longValue;
}
```
2. `${...}` 사용: properties 값 주입
	* 해당 속성값은 런타임에 변수로 저장되며, 속성값이 properties 파일에 없는 경우 에러 발생
```properties
# application.properties
hello.message=Hello World!
```
```java
public class TestClass {
	@Value("${hello.message}")
	private String helloWorld;
}
```
```java
public class TestService {
	private String val1;
	private String val2;
	
	// 파라미터로 넘기면서 주입할 수 있다.
	public TestService(
		@Value("val1") String val1,
		@Value("val2") String val2
	) {
		this.val1 = val1;
		this.val2 = val2;
	}
}
```
3. `${...}` 사용하여 List 주입
```properties
# application properties
color.list=red,blue,green
```
```java
public class TestClass {
	@Value("${color.list}")
	private List<String> colorList;
}
```
4. `#{${...}}` 사용하여 Map 주입
```properties
# application properties
my.config={url:'http://localhost:8081', profile:'local', key:'123456'}
```
```java
public class TestClass {
	@Value("#{${my.config: {url:'http://localhost:8081', profile:'local', key:'123456'}}}")
	private Map<String, String> configMap;
}
```


