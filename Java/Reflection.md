# Reflection
### Reflection 이란?
* Reflection은 실행 중인 Java 프로그램에서 클래스, 인터페이스, 필드, 메서드 등의 정보를 가져오고, **이런 요소들을 동적으로 조작할 수 있는 기능**이다.
* Reflection을 사용하면 **컴파일 시점에 알 수 없었던** 클래스의 정보를 조사하고, 객체를 생성하며, 메서드를 호출하고, 필드에 접근할 수 있다.
### Reflection 사용해야 하는 경우
1. **클래스의 정보 분석**
    * Reflection을 사용하여 실행 중인 Java 프로그램에서 클래스의 이름, 상속 계층, 구현 인터페이스, 필드 및 메서드 등의 정보를 동적으로 알아낼 수 있다.
    * 이를 활용해 클래스의 **동적 로딩, 메타데이터 검사, 동적 바인딩** 등 여러 작업이 수행 가능하다.
2. **객체의 동적 생성**
   * Reflection을 사용하면 실행 중에 동적으로 객체를 생성할 수 있다.
   * 클래스 이름을 동적으로 알아내고, `Class.newInstance()` 메서드를 사용하여 객체를 생성할 수 있다.
   * **팩토리 패턴, 의존성 주입** 등의 상황에서 유용할 수 있다.
3. **메서드의 동적 호출**
   * Reflection을 사용하면 실행 중에 메서드를 동적으로 호출할 수 있다.
   * 메서드 이름, 매개변수 타입 등의 정보를 알아내고, `Method.invoke()` 메서드를 사용하여 메서드를 호출할 수 있다.
   * Reflection 기반의 동적 디스패치 등의 상황에서 유용할 수 있다.
### Reflection의 내장함수
* 아래 함수들은 `java.lang.Class` 클래스의 메서드로 제공한다.
1. `getDeclaredFields()`
    * 클래스의 모든 필드를 반환한다.
    * 상속받은 필드나 인터페이스에서 상속받은 필드는 포함하지 않는다.
    * `Field` 배열로 반환되며, 각 필드에는 해당 필드의 정보와 기능에 접근하기 위한 메서드가 있다.
2. `getDeclaredMethods()`
    * 클래스의 모든 메서드를 반환한다.
    * 상속받은 메서드나 인터페이스에서 상속받은 메서드는 포함하지 않는다.
    * `Method` 배열로 반환되며, 각 메서드에는 해당 메서드의 정보와 기능에 접근하기 위한 메서드가 있다.
3. `getDeclaredConstructors()`
    * 클래스의 모든 생성자를 반환한다.
    * `Constructor` 배열로 반환되며, 각 생성자에는 해당 생성자의 정보와 기능에 접근하기 위한 메서드가 있다.
4. `getMethod(String name, Class<?>... parameterType)`
   * 지정된 이름과 매개변수 타입에 해당하는 공개 메서드를 반환한다.
   * 상속받은 메서드 또한 포함한다.
   * `Method` 객체를 반환하며, 해당 메서드에는 해당 메서드의 정보와 기능에 접근하기 위한 메서드가 있다.
5. `getField(String name)`
   * 지정된 이름의 공개 필드를 반환한다.
   * 상속받은 필드 또한 포함한다.
   * `Field` 객체를 반환하며, 해당 필드에는 해당 필드의 정보와 기능에 접근하기 위한 메서드가 있다.
* 이 외에도 `getSuperclass()`, `getInterface()`, `newInstance()` 등의 메서드를 사용하면 클래스의 상위 클래스, 구현한 인터페이스, 객체의 인스턴스를 생성하는 등의 동작을 수행할 수 있다.
### Reflection 주의 사항
1. 성능
   * Reflection은 일반적인 메서드 호출보다 성능이 떨어질 수 있다.
   * Reflection 작업은 추가적인 오버헤드가 발생하므로 성능이 중요한 코드에서 너무 자주 사용하는 것은 좋지 않다.
2. 보안
   * Reflection을 사용하면 접근 제한자를 우회하여 private 멤버에 접근할 수 있다.
   * Reflection을 사용할 때는 보안 검사를 충분히 수행하고 필요한 권한을 확인해야 한다.
3. 가독성 및 유지보수성
   * Reflection은 코드의 가독성과 유지보수성을 저하시킬 수 있다.
   * Reflection을 사용하면 코드의 동작이 동적이 되므로 코드를 이해하기 어려워질 수 있다.
   * Reflection을 사용할 때는 명확한 이유와 목적이 있을 경우에만 사용하고, 주석이나 문서화를 통해 코드의 의도를 명확히 전달해야 한다.
### 예제 소스
```java
public class ReflectionExample {
	public static void main(String[] args) throws Exception {
       MyClass instance = new MyClass();
		
		// 클래스 정보 가져오기
       Class<?> clazz = MyClass.class;
	   
	   // 필드에 접근하여 값 설정하기
       Field field = clazz.getDeclaredField("myField");
	   field.setAccessible(true);   // 접근 권한 설정
	   field.set(instance, "New Value");
	   
	   // 메서드 호출하기
       Method method = clazz.getDeclaredMethod("myMethod");
	   method.setAccessible(true);
	   method.invoke(instance);
    }
}

class MyClass {
	private String myField;
	
	private void myMethod() {
       System.out.println("My Method.");
    }
}
```
* `MyClass` 클래스의 필드(`myField`)와 메서드(`myMethod`)에 접근하여 값을 설정하고 메서드를 호출한다.
* `Class` 객체를 사용하여 클래스 정보를 가져오고, `Field`와 `Method` 객체를 사용하여 필드와 메서드에 접근한다.
* `setAccessible(true)`로 접근 권한을 설정해야 private 멤버에 접근 가능하다.
