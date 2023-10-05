# Spring Cloud Gateway
* Spring Cloud Gateway 는 Spring 프레임워크 기반의 마이크로서비스 아키텍처에서 사용되는 API 게이트웨이다.
* 요청 라우팅, 필터링, 보안, 로깅 등을 중앙 집중적으로 관리할 수 있으며, 여러 마이크로서비스의 엔드포인트를 하나의 진입점으로 노출시킬 수 있다.

### Spring Cloud Gateway 의 주요 특징
1. **루프백 방지**
    * 무한 루프가 발생하지 않도록 설계되어 있다.
2. **필터**
    * 요청과 응답을 조작하기 위한 필터 체인 제공
    * 이를 사용하여 로깅, 보안, 인증, 요청/응답 변환 등을 수행할 수 있다.
3. **로드 밸런싱**
    * 여러 인스턴스로 구성된 백엔드 서비스에 요청을 분산하는 로드 밸런싱 기능을 제공
4. **서킷 브레이커**
    * 서비스 간의 의존성 관리를 개선하기 위해 서킷 브레이커 패턴 지원
5. **HTTP/2 지원**
    * HTTP/2를 지원하여 효율적인 통신을 가능하도록 한다.
6. **도커 및 쿠버네티스 통합**
    * 컨테이너 환경에서 사용하기 쉽도록 설계되어 있으며, 쿠버네티스와 통합하여 사용할 수 있다.

### 오픈소스 Gateway Service 비교
|   |Kong|Zuul API Gateway|Spring Cloud Gateway|
|---|---|---|---|
|Database 지원|Postgresql, Cassandra|Mysql, Cassandra, Elastic Search|제약 없음|
|구현 기술|Nginx, Lua|Java|Java|
|인증 및 보안<br/>(OAuth2.0, jwt)|활용 가능|활용 가능|활용 가능
|쿠버네티스|활용 가능|활용 가능|활용 가능|
|로드 밸런싱|지원|지원|지원|
|모니터링 및 로깅|지원|지원|지원|
|플러그인|다양한 플러그인 지원|X|다양한 플러그인 지원|
|UI|O|X|O|
|장점|설치, 환경 구성 용이<br/>Nginx 기반으로 성능 우수<br/>Admin UI 지원|많은 서비스에서 검증됨<br/>Ver 2: None-Blocking IO (성능 우수)|Spring Cloud 생태계 (최신 트렌드 적용)<br/>Predicate, Filter 지원<br/>None-Blocking IO (성능 우수)|
|단점|안정된 라이브러리 수 적음<br/>생태계가 작음|Ver 1: Blocking IO<br/>기능 추가 공식적으로 중단 선언|Java 환경이 아닌 경우 지원 라이브러리 확인 필요|

### Spring Cloud Gateway 사용 예제
1. Maven 의존성 추가
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId>
    </dependency>
</dependencies>
```

2. 게이트웨이 라우팅 및 필터 구성 예제
```java
import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class GatewayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
            .route("example", r -> r
                .path("/example/**")
                .uri("http://example.com")) // example.com으로 라우팅
            .route("google", r -> r
                .path("/google/**")
                .filters(f -> f.rewritePath("/google/(?<segment>.*)", "/${segment}"))
                .uri("https://www.google.com")) // google.com으로 라우팅, 경로 재작성
            .build();
    }
}
```
* 위 예제에서는 `/example/**` 경로로 요청이 오면 `http://example.com`으로 요청을 라우팅하고, `/google/**` 경로로 요청이 오면 `http://www.google.com`으로 요청을 라우팅하면서 경로를 재작성한다.

