# 303 Code
* `303 See Other`
* 다른 위치로 요청하라

### 원인
* 주로 HTML 폼을 사용하는 웹 애플리케이션에서 POST 요청 이후 사용된다.
* 이때, 서버는 POST 요청을 처리하고 적절한 응답을 생성 후, 클라이언트에게 새로운 리소스 위치로 이동하라는 신호로 `300 See Other`를 반환한다.

### 조치사항
* 클라이언트는 새로운 위치로 GET 요청을 보내 리소스를 가져와야 한다.
* 이를 통해 브라우저는 사용자에게 적절한 리소스를 표시할 수 있다.

### 정리
* `303 See Other`는 POST 요청 이후에 사용되며, 클라이언트에게 새로운 리소스 위치로 이동하라는 신호를 주는데, 클라이언트는 이에 따라 GET 요청을 보내 새로운 리소스를 가져와야 한다.

### 예시
```bash
POST /submit-form HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
...
(form data)

HTTP/1.1 303 See Other
Location: /thank-you
```
* 위 예시에는 사용자가 어떤 폼을 제출한 후, 서버는 폼을 처리하고 `/thank-you`라는 새로운 위치로 리다이렉션하는 `303` 상태 코드를 반환한다.
* 클라이언트는 이후에 `/thank-you`로 GET 요청을 보내 결과를 확인한다.
