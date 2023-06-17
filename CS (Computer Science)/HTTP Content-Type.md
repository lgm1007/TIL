# HTTP Content Type
### Content-Type
* 보통 클라이언트와 서버 간 전송되는 **데이터가 어떤 유형**인지 명시하는 역할을 한다.
* 이를 통해 수신측은 데이터를 올바르게 해석하고 처리할 수 있다.
### HTTP Request 구조
1. Request Line
2. HTTP Header
3. Empty Line
4. Message Body

* 위 구조에서 **Message Body**에 들어가는 타입을 **HTTP Header**에 명시해줄 수 있다.
  * 명시해주도록 해주는 필드가 Content-Type
### Content Type 유형 예시
* 텍스트 타입
  * `text/css`, `text/html`, `text/plain`
* 파일 타입
  * `multipart/form-data`
* 애플리케이션 타입
  * `application/json`, `application/x-www-form-urlencoded`
### application/json 과 application/x-www-form-urlencoded
1. `application/json`
   * **JSON 형식의 데이터**를 나타내는 데 사용되는 MIME 타입
     * MIME : Multipurpose Internet Mail Extensions 로 이메일과 함께 동봉할 파일을 텍스트 문자로 전환하기 위해 개발되었으며, 현재는 **웹을 통해 여러 형태의 파일 전달을 위해** 사용되고 있다.
   * 주로 API 엔드포인트에서 데이터를 전송하거나 수신할 때 사용된다.
   * JSON 데이터는 중괄호({})로 둘러쌓여 있는 키-값 쌍의 집합으로 표현된다.
```
Content-Type: application/json

{
  "name": "John Doe",
  "age": 25,
  "email": "johndoe@example.com"
}
```
2. `application/x-www-form-urlencoded`
   * **웹 폼(form) 데이터를 URL 인코딩하여 전송**하기 위한 MIME 타입
   * 웹 폼 데이터를 서버로 전송할 때 주로 사용된다.
   * 데이터는 key=value 형식으로 인코딩되며, 각 키-값 쌍은 `&` 기호로 구분된다.
```
Content-Type: application/x-www-form-urlencoded

name=John+Doe&age=25&email=johndoe@example.com
```
* 둘의 주요 차이점은 **데이터 형식**과 **사용 목적**이다.
  * `application/json` 은 **API 통신이나 데이터 교환**에 주로 사용된다.
    * 즉, 구조화된 데이터를 표현하고 교환하는 데 적합하다.
  * `application/x-www-form-urlencoded` 는 주로 **웹 폼을 통한 데이터 전송**에 사용한다.
    * 즉, 폼 데이터를 표현하고 전송하는 데 적합하다.
