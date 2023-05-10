# 스프링의 HTTP 클라이언트 라이브러리
* 스프링에서 REST API를 호출하는 방법
### RestTemplate
##### 특징
* Blocking I/O 방식을 사용하며 동기 방식으로 동작한다.
  * 요청과 응답이 완료될 때 까지 대기
* 다양한 HTTP 메서드 (GET, POST, PUT, DELETE 등) 를 지원하며, 응답을 도메인 객체로 매핑하는 기능을 제공한다.
* 응답을 도메인 객체로 매핑하는 기능을 제공한다.
* Interceptor를 사용하여 HTTP 요청과 응답을 수정하고, 로깅 등의 추가적인 작업을 수행할 수 있다.
##### 장단점
* 장점: 사용이 간단하다는 점
* 단점: Blocking I/O 방식을 사용하기 때문에 요청이 많아질 경우 성능 이슈가 발생할 수 있다.
##### 사용 예제
* RestTemplate 를 사용한 **GET** 요청
```java
RestTemplate restTemplate = new RestTemplate();
String url = "https://jsonplaceholder.typicode.com/posts/1";
Post post = restTemplate.getForObject(url, Post.class);
```
* RestTemplate 를 사용한 **POST** 요청
```java
RestTemplate restTemplate = new RestTemplate();
String url = "https://jsonplaceholder.typicode.com/posts/1";
String requestBody = "{ \"key1\": \"value1\", \"key2\": \"value2\" }";
HttpHeaders headers = new HttpHeaders();

headers.setContentType(MediaType.APPLICATION_JSON);
HttpEntity<String> request = new HttpEntity<>(requestBody, headers);
ResponseEntity<String> response = restTemplate.postForEntity(url, request, String.class);
```
### WebClient
##### 특징
* Non-Blocking I/O 방식을 사용하여 비동기 방식으로 동작한다.
* 요청과 응답이 완료되지 않은 상태에서 다른 요청을 보낼 수 있으며, 이를 통해 동시에 여러 요청을 처리할 수 있다.
* Reactor 라이브러리의 Mono와 Flux 타입을 반환하며, 이를 통해 비동기 처리를 구현할 수 있다.
##### 장단점
* 장점: Non-Blocking I/O 방식을 사용하여 높은 성능을 기대할 수 있다.
* 단점: 사용이 복잡하고 익숙하지 않은 개발자에게는 어려울 수 있다.
##### 사용 예제
* WebClient 를 사용한 **GET** 요청
```java
WebClient webClient = WebClient.builder().baseUrl("https://jsonplaceholder.typicode.com").build();
Mono<Post> postMono = webClient.get()
        .uri("/posts/{id}", postId)
        .retrieve()
        .bodyToMono(Post.class);

Post post = postMono.block();
```
* WebClient 를 사용한 **POST** 요청
```java
WebClient webClient = WebClient.builder().baseUrl("https://jsonplaceholder.typicode.com").build();
Mono<Post> postMono = webClient.post()
        .uri("/posts")
        .contentType(MediaType.APPLICATION_JSON)
        .body(BodyInserters.fromValue(post))
        .retrieve()
        .bodyToMono(Post.class);

Post createdPost = postMono.block();
```
### 비교
* **RestTemplate**
  * 사용이 간편하여, 동기적인 방식으로 처리한다.
* **WebClient**
  * 비동기 방식으로 처리하여, 높은 성능을 기대할 수 있다.

