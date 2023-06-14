# Checked Exception 대하기
### Checked Exception 과 Unchecked Exception 차이
|-|Checked Exception|Unchecked Exception|
|---|---|---|
|처리 여부|반드시 예외처리 해야 함|예외처리 하지 않아도 됨|
|대표 Exception|IOException <br/> SQLException|NullPointerException <br/> IllegalArgumentException|
* Unchecked Exception은 **RuntimeException을 상속**
* Checked Exception은 RuntimeException을 **상속하지 않음**
### Checked Exception
* Checked Exception은 기본적으로 복구가 가능하다는 메커니즘을 가짐
	* 하지만 일반적으로 Checked Exception이 발생했을 경우 복구 전략을 갖고 복구할 수 있는 경우는 많지 않음
    * 예시) Unique 해야 할 아이디 값이 중복되어 SQLException 발생 시 어떤 복구 전략을 가질 수 있는가?
      * 그냥 RuntimeException 발생시킨 후 유저에게 입력을 다시 유도하는 편이 나은 전략이 될 수 있다.
* Exception을 발생시킬 때 **어떤 예외가 발생해서 Exception이 발생한 것인지 명확한 정보**를 전달해주는 것이 중요
  * 위 예시에서는 DuplicateUniqueException (Unchecked Exception)을 발생시키기
  * 즉 **더 구체적인 Unchecked Exception을 발생시켜야 함**
### 정리
* 예외 복구 전략이 명확하고 그것이 가능하다면 Checked Exception을 try catch 로 잡고 복구를 하는 것이 좋다.
* 하지만 예외 복구 전략이 가능한 경우는 흔치 않으며 **Checked Exception이 발생하면 더 구체적인 Unchecked Exception을 발생시키고 예외에 대한 메시지를 명확하게 전달하는 것이 더 효과적**이다.
* 무책임하게 상위 메서드로 throw 하는 행위는 좋지 않다.
* Checked Exception 은 기본 트랜잭션의 속성에서는 Rollback을 진행하지 않는 점도 알고 있어야 실수를 방지할 수 있다.

