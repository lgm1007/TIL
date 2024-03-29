# 405 Code
* `405 Method Not Allowed`
* 요청한 URI가 지정한 메서드를 지원하지 않는다.

### 원인
* 클라이언트가 요청한 리소스는 존재하지만, 해당 리소스는 클라이언트가 사용한 HTTP 메서드를 처리할 수 없는 경우 발생
  * 예를 들어, GET 메서드만 허용된 리소스에 대해 POST 요청을 보낼 경우 405 코드가 발생할 수 있다.

### 해결 방안
* **올바른 HTTP 메서드 사용**
  * 요청한 리소스가 지원하는 HTTP 메서드를 사용하는지 확인
* **허용된 메서드 확인**
  * 서버의 Allow 헤더를 확인하여 허용된 메서드 목록 확인
* **서버 설정 확인**
  * 서버 측에서 특정 메서드를 처리하도록 설정되어 있는지 확인
  * 서버 구성 파일이나 코드 검토

### 정리
* 클라이언트 요청한 리소스에서 허용되지 않은 HTTP 메서드를 사용했을 때 발생하는 상태 코드로, 올바른 메서드를 사용하거나 서버 설정을 수정하여 해결할 수 있다.
