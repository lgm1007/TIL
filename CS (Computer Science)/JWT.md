# JWT (Json Web Token)
### JWT 란?
* 웹 및 애플리케이션 간에 정보를 안전하게 전송하기 위한 토큰 기반의 인증 방식 중 하나
* Json 형식에 인증에 필요한 정보들을 담은 후 비밀키로 서명한 토큰이며, [인터넷 표준 인증 방식](https://datatracker.ietf.org/doc/html/rfc7519)이다.
* 공식적으로 인증 (Authentication)과 권한 (Authorization) 방식으로 사용된다.

### JWT의 구조
1. **헤더** (Header)
    * 토큰의 유형 및 사용되는 해시 알고리즘과 같은 메타데이터를 정의하는 부분
2. **페이로드** (Payload)
    * 클레임 (claim) 정보가 저장되는 부분
      * 클레임: 사용자에 대한 정보나 권한에 대한 주장. 예를 들어 사용자 ID, 역할, 만료 시간 등이 포함
    * 실제로 전송하려는 데이터가 페이로드에 들어간다.
3. **서명** (Signature)
    * 헤더와 페이로드의 문자열을 합친 후에, 헤더에서 선언한 알고리즘과 key를 이용해 암호한 값

### JWT 구조 예제
* JWT의 일반적인 형태
  * ```
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
    ```
* 헤더
  * ```
    {
        "alg": "HS256",
        "typ": "JWT"
    }
    ```
  * `alg`: 서명에서 사용하는 알고리즘
  * `typ`: 토큰 타입
* 페이로드
  * ```
    {
        "sub": "1234567890",
        "name": "Example Name",
        "iat": 1516239022,
        "exp": 1516259526
    }
    ```
  * `sub`: 토큰 제목 (subject)
  * `name`: 토큰 대상자 (audience)
  * `iat`: 토큰이 발급된 시각 (issue at)
  * `exp`: 토큰의 만료 시각 (expired)
* 서명
  * ```
    SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
    ```

### JWT 프로세스
1. **사용자 인증**
    * 사용자가 자격 증명 (일반적으론 사용자 이름과 암호) 으로 서버에 로그인 시도
    * 서버는 사용자의 자격 증명을 확인하고, 유효한 경우 사용자에 대한 정보를 기반으로 JWT를 생성
2. **JWT 생성**
   * 서버는 해더, 페이로드 및 서명으로 구성된 JWT를 생성
3. **JWT 전송**
    * 서버는 생성된 JWT를 클라이언트에게 전달
    * 일반적으로 HTTP 응답 헤더 또는 JSON 응답의 일부로 전송된다.
4. **클라이언트 요청과 JWT 사용**
    * 클라이언트는 인증이 필요한 요청을 보낼 때마다 JWT를 요청에 포함시켜 서버에 전달한다.
    * 일반적으로 HTTP 헤더의 `Authorization` 필드에 `Bearer` 스킴을 사용하여 JWT를 전송
      * ```
        Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
        ```
    * 서버는 요청을 수신하고, JWT의 유효성을 검사한다.
      * 서명 확인, 만료 시간 등 클레임 확인 등
5. **서버 응답**
    * 서버는 클라이언트의 요청을 처리하고 응답을 생성
    * 필요한 경우에는 새로운 JWT를 생성하여 클라이언트에게 전달할 수도 있다.

### JWT의 사용 시나리오
* **인증**
  * 사용자가 로그인하면 서버는 JWT를 생성하고 클라이언트에게 전달
  * 이후 클라이언트는 각 요청에 JWT를 포함하여 서버에 자격 증명을 제공
* **정보 교환**
  * 두 시스템 간에 정보를 안전하게 교환할 때 JWT가 사용될 수 있다.
* **클레임 기반 접근 제어**
  * JWT는 클레임을 통해 특정 자원에 대한 액세스 권한을 부여하고 관리하는 데 사용될 수 있다.

### JWT의 장단점
* **장점**
  * 로컬에 저장하기 때문에 서버 용량에 영향을 미치거나 하지 않는다.
  * 공개키, 개인키 또는 비밀키를 통해 서명되기 때문에 비교적 안전하다.
  * 모바일 앱에서 사용하기 적합하다.
    * 모바일 앱은 여러 플랫폼 또는 기기에서 동작할 수 있고, 서로 다른 도메인에서 통신할 수 있기 때문에 JWT를 사용하여 플랫폼 독립적으로 사용자 인증 처리를 할 수 있다.
  * 네트워크 부하가 적다.
    * HTTP 헤더 또는 URL 파라미터를 통해 간단하게 전송되기 때문이다.
* **단점**
  * 토큰의 크기가 커질수록 트래픽에 영향을 미칠 수 있다.
  * 토큰이 발급되면 만료 기간 변경이 불가능하므로 토큰 만료 처리를 꼭 구현해야 한다.

#### 매번 요청때마다 JWT를 전달해줘야 하는 번거로음
* 매번 요청 때마다 JWT 인증 절차를 거치는 것은 번거로울 뿐 아니라 통신이 느려지는 문제가 될 수도 있다.
* HTTP 프로토콜 통신은 `connectionless`하고 `stateless`한 특성 때문에 매 통신 때마다 JWT 인증 절차를 거치는 것
  * 한 번 통신이 일어나면 연결이 끊어지고 다시 연결해도 이전 상태를 유지하지 않는다는 특성
* 인증된 사용자가 어느 정도 기간동안까진 재인증하지 않아도 되도록 만든 것이 `Access Token`이다.


