# HTTP의 이해
### HTTP (HyperText Transfer Protocol) 란?
* HTML 파일(`HyperText`)을 전송(`Transfer`)하기 위한 컴퓨터끼리 어떻게 HTML 파일을 주고 받을지에 대한 약속이자 애플리케이션 레이어의 프로토콜(`Protocol`)
  * **Protocol**: 상호 합의된 규칙, 데이터 통신을 원활하게 하기 위해 필요한 통신 규칙
* HTTP는 신호 송신의 순서, 데이터의 표현법 등을 정함
### HTTP 특징
1. **HTTP request & HTTP response**
![http_request_response](https://velog.velcdn.com/images/tiger/post/f9e95a0e-ead5-4e3e-bb83-dd254d788a04/image.png)
<br/>
<br/>
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


