# Spring
### Spring 이란?
* Java의 웹 프레임워크
  * Java 언어로 다양한 애플리케이션을 만들기 위한 프로그래밍 툴
  * Java를 이용한 기술인 JSP, MyBatis, JPA 등 프로젝트에 많이 쓰이는 기술들을 더 편하게 사용하기 위해 만들어짐
### Spring의 주요 특징
1. **IoC** (Inversion of Control, 제어 반전)
    * 인스턴스(객체)의 생성부터 소멸까지 **객체 생명주기 관리를 개발자가 하는 게 아닌 컨테이너가 대신 해주는 것**을 의미
    * IoC가 개발자의 코드를 호출하여 그 코드로 생명주기 제어
2. **DI** (Dependency Injection, 의존성 주입)
    * 프로그래밍에서 구성요소 간 **의존 관계**가 소스코드 내부가 아닌 **외부의 설정 파일을 통해 정의**되는 방식
    * 코드 재사용을 높여 소스코드를 다양한 곳에서 사용할 수 있도록 하며 모듈 간 결합도를 낮춤
3. **AOP** (Aspect Object Programming, 관점 지향 프로그래밍)
    * 로깅, 트랜잭션, 보안 등 여러 모듈에서 **공통적으로 사용되는 기능을 분리하여 관리**하는 것
    * **여러 객체에 공통으로 적용할 수 있는 기능을 구분함으로써 재사용성을 높여주는 프로그래밍 기법**
4. **POJO 방식** (Plain Old Java Object)
    * Java EE를 사용하면서 해당 플랫폼에 종속되어 있는 무거운 객체들을 만드는 것에 반발하여 나타난 용어
    * 별도의 프레임워크 없이 Java EE를 사용할 때에 비해 인터페이스를 직접 구현하거나 상속받을 필요가 없어 기존 라이브러리를 지원하기 용이하고, 객체가 가벼움
    * **getter/setter를 가진 단순한 Java 오브젝트**
