# HTTP의 이해
### HTTP (HyperText Transfer Protocol) 란?
* HTML 파일(`HyperText`)을 전송(`Transfer`)하기 위한 컴퓨터끼리 어떻게 HTML 파일을 주고 받을지에 대한 약속이자 애플리케이션 레이어의 프로토콜(`Protocol`)
  * **Protocol**: 상호 합의된 규칙, 데이터 통신을 원활하게 하기 위해 필요한 통신 규칙
* HTTP는 신호 송신의 순서, 데이터의 표현법 등을 정함
### HTTP 특징
1. **HTTP request & HTTP response**
![http_request_response](https://velog.velcdn.com/images/tiger/post/f9e95a0e-ead5-4e3e-bb83-dd254d788a04/image.png)  
* request : 해당 사이트를 브라우저에 표현하기 위해 해당 사이트의 자료를 달라고 요청하는 것
* response : 요청받은 해당사이트 관련 자료들을 브라우저에게 응답으로 전달해주는 것
2. **stateless**
    * State(상태) + less(없음)
    * 각각의 HTTP 통신(요청/응답)은 독립적이기 때문에 과거의 통신에 대한 내용을 전혀 알지 못함
    * 이전의 상태를 전혀 알지 못한다는 것 = 이전에 준 정보를 전혀 알지 못한다는 것을 의미
    * 따라서, 여러번의 통신의 진행과정에서 연속된 데이터 처리가 필요한 경우를 위해 **로그인 토큰** 또는 **브라우저의 쿠키, 세션, 로컬스토리지** 같은 기술이 필요에 의해 만들어짐
    * stateful 사용을 안하고 **stateless 사용하는 이유**
      * 채팅과 같이 내용을 보존해야 하는 경우는 stateful 사용하지만,
      * 서버가 많은 데이터를 가지고 있으면 서버가 무거워지고 그로 인해 요청을 빨리 처리하기 힘들기 때문에 stateless 사용
### Request / Response
1. HTTP request
    * **형식**
      * **Start Line**: 요청의 첫 번째 줄
        * HTTP Method
        * Request target
        * HTTP Version
      * **Headers**: 해당 요청에 대한 추가 정보(메타 데이터)를 담고 있는 부분
        * Host: 요청을 보내는 타겟의 주소. 요청을 보내는 웹사이트의 기본 주소가 됨 (ex. www.google.co.kr)
        * User-Agent: 요청을 보내는 클라이언트(웹 브라우저)에 대한 정보 (ex. chrome, firefox, safari)
        * Accept-Encoding: 웹 브라우저에서 가능한 압축방식. 서버는 브라우저가 알려준 압축방식들 중에서 가능한 압축방식을 골라서 사용함
        * Content-Type: 해당 요청이 보내는 메시지 body의 타입 (ex. application/json)
        * Authorization: 회원의 인증/인가를 처리하기 위해 로그인 토큰을 Authorization에 담음
        * If-Modified-Since: 현재 파일이 언제 마지막으로 다운받은 파일인지를 나타냄
          * 서버는 자신이 가지고 있는 파일과 비교하여 무엇이 더 최신인지를 알아내고 서버가 가지고 있는 것이 최신이면 서버의 파일을 브라우저에게 전송함
          * 서버의 파일보다 브라우저의 파일이 최신이거나 같다면 불필요한 다운을 하지 않기 위해 전송하지 않음
      * **Body**: 해당 요청의 실제 내용, 주로 POST 메서드가 사용
        * (ex. 로그인 시에 서버에 보낼 요청의 내용)
	```json
	Body: {
		"user_email": "example@email.com",
		"user_password": "helloworld"
	}
	```

2. HTTP response
    * **형식**
      * **Start Line**
        * HTTP Version: 요청의 HTTP 버전
        * status code: 응답 결과 (상태코드)
        * phrase: 응답 결과를 사람이 이해하기 쉽도록 말로 풀어 쓴 것
      * **Headers**: 요청의 헤더와 동일
        * 응답에서만 사용되는 헤더의 정보 (ex. 요청하는 브라우저의 정보가 담긴 *User-Agent 대신 Server 헤더*가 사용됨)
      * **Body**: 요청의 Body와 일반적으로 동일
        * 가장 많이 사용되는 Body의 데이터 타입은 JSON
        * (ex. 로그인 요청이 성공할 때 응답의 내용)
   ```json
   Body: {
	   "message": "SUCCESS",
	   "token": "j0fej92308u9d"
   }
	```
### HTTP Request Methods
* 프론트엔드에서 요청의 의도를 담기 위해 **HTTP 요청 메서드**(Http Request Methods)를 사용함
1. **GET**: 자료를 요청
    * 데이터를 서버로부터 받아올 때 사용
    * 데이터를 받아오기만 할 때 주로 사용
2. **POST**: 자료 생성을 요청
    * 데이터 생성 수정에 사용하므로 대부분 body가 포함되어 보내짐
3. **PUT**(전체), **PATCH**(일부): 데이터의 수정을 요청
4. **DELETE**: 서버 데이터 삭제를 요청
### HTTP 상태코드
1. **1XX** (조건부 응답): 요청을 받았으며 작업을 계속한다.
2. **2XX** (성공): 클라이언트의 요청을 성공적으로 처리하였음을 가리킨다.
    * 200: OK
      * 문제없이 요청에 대한 처리가 백엔드 서버에서 이루어지고 나서 오는 응답코드
    * 201: Created
      * 무언가 잘 생성되었을 때 오는 응답코드
      * 대개 POST 요청에 따라 백엔드 서버에 데이터가 잘 생성 또는 수정되었을 때 보내지는 코드
    * 204: No Content
      * 요청이 성공했으며 제공할 응답 메시지가 없을 경우 사용되는 상태코드
      * 대개 DELETE 요청으로 데이터가 성공적으로 삭제되어 응답으로 제공할 콘텐츠가 없을 때 사용
3. **3XX** (리다이렉션 오류): 클라이언트는 요청을 마치기 전에 추가 조치가 필요하다.
4. **4XX** (요청 오류): 클라이언트에 오류가 있음을 나타낸다.
    * 400: Bad Request
      * 주로 요청이 잘못 되었을 때 보내지는 상태코드
    * 401: Unauthorized
      * 유저가 해당 요청을 진행하려면 먼저 로그인을 하거나 회원가입이 필요하다는 의미를 나타내는 상태코드
    * 403: Forbidden
      * 유저가 해당 요청에 대한 권한이 없다는 의미를 나타내는 상태코드
      * 접근 불가능한 정보에 접근했을 경우를 의미
      * (ex. 오직 유료회원만 접근할 수 있는 데이터를 요청했을 경우)
    * 404: Not Found
      * 요청한 URI가 존재하지 않다는 의미를 나타내는 상태코드
5. **5XX** (서버 오류): 서버가 유효한 요청을 수행하는 것에 실패했다.
    * 500: Internal Server Error
      * 서버에서 에러났을 때의 상태코드
### HTTP VS HTTPS
* HTTP가 처음 나왔을 땐 세상에 존재하지 않은 통신으로 많은 정보들을 나누지 않았다. 따라서 HTTP는 **암호화기능을 가지고 있지 않다**.
* **HTTPS** (HTTP over Secure socket layer) 으로 HTTP에 **데이터 암호화**가 추가된 프로토콜로 안전한 HTTP라고 볼 수 있다.
* SSL (Secure Socket Layer)
  * 데이터를 암호화하여 인터넷 연결을 보호하기 위한 표준 기술
* HTTPS로 통신하고 있으면 누가 들여다본다고 해도 데이터가 암호화가 되어있어 무슨 내용인지 알 수 없다.
### HTTP 통신 모니터링 도구
* 브라우저 개발자 도구 (Developer tools) > Network
* Wireshark

