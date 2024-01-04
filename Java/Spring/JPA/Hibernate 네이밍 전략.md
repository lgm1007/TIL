# Hibernate 네이밍 전략
### JPA Entity 생성 시 변수 네이밍
* Hibernate에서 기본값으로 JPA 적용 후 엔티티 생성 시 언더바 형식으로 변환한다.
  * ex) UserName -> user_name
* 이런 이유로 데이터베이스의 테이블과 엔티티가 매핑되지 않는 문제가 발생할 수도 있다.
  * 만약 데이터베이스 테이블 명이나 컬럼명을 카멜 케이스로 작성했다면, 엔티티는 언더바 형식으로 매핑을 하려 하기 때문에 매핑되지 않는다.

### 네이밍 전략 단계
1. **적절한 논리적 이름(logical name) 결정**
   * 논리적 이름은 개발자가 명시적으로 `@Table(name=""), @Column(name="")`를 작성하거나, 암시적으로 `ImplicitNamingStrategy`를 통해 결정할 수 있다.
2. **논리적 이름을 물리적 이름으로 변환**
   * `PhysicalNamingStrategy`에 정의된 전략을 통해 물리적 이름으로 변환
   * 기본값은 앞의 논리적 이름 사용

#### PhysicalNamingStrategy
* 논리적 JPA 객체 이름에 기반하여 실제 데이터베이스에 사용되는 물리적 이름을 제공한다.
* 기본적으로는 카멜 케이스를 언더바 형식으로 바꾸며, 엔티티의 변수명을 그대로 사용하고 싶다면 Hibernate 설정에 아래와 같이 설정한다.
```yaml
# application.yml
spring:
  jpa:
    hibernate:
      naming:
        physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
```

