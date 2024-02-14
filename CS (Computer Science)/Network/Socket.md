# 소켓 (Socket)
* 네트워크를 경유하는 프로세스 간 통신의 종착점
* OSI 7 계층에서 전송 계층과 세션 계층 사이에 위치한다.
* 7 계층 중 응용 계층에 속하는 프로세스들은 데이터 송수신을 위해 반드시 소켓을 거쳐 전송 계층으로 데이터를 전달해야 한다.
  * 소켓은 전송 계층과 응용 계층 사이의 인터페이스 역할을 하며 떨어져 있는 두 호스트를 연결해준다.

### 소켓 구성요소
* 인터넷 프로토콜 (TCP, UDP, raw IP)
* 로컬 IP 주소
* 로컬 포트
* 원격 IP 주소
* 원격 포트

### 소켓 통신의 일반적인 흐름
* **서버**
  * `socket()`: 소켓 생성
  * `bind()`: 바인딩 (IP, Port 번호 설정)
  * `listen()`: 클라이언트 요청에 대기열을 만들어 몇 개의 클라이언트를 대기시킬지 결정
  * `accept()`: 클라이언트와 연결
  * `recv(), send()`: 데이터 송수신
  * `close()`: 소켓 닫기
* **클라이언트**
  * `socket()`: 소켓 생성
  * `connect()`: 서버에 설정된 IP, Port로 연결 시도
  * `recv(), send()`: 데이터 송수신
  * `close()`: 소켓 닫기

### HTTP 통신과 소켓 통신의 차이
1. **HTTP 통신**
  * 클라이언트의 요청이 있을 때만 서버가 응답한다.
  * 다양한 데이터 타입을 주고받을 수 있다.
    * ex) JSON, HTML, Image
  * 서버가 응답한 후 연결을 바로 종료하는 단방향 통신, Keep Alive 옵션을 주어 일정 시간동안 커넥션을 유지할 수 있다.
  * 실시간 연결이 아닌 데이터 전달이 필요한 경우에만 요청을 보내는 상황에 유리하다.
2. **소켓 통신**
   * 클라이언트와 서버가 특정 포트를 통해 양방향 통신
     * 데이터 전달 후 연결이 끊어지는 것이 아니라 계속해서 연결 유지
     * HTTP 통신보다 더 많은 리소스를 소모한다.
   * 실시간 동영상 스트리밍이나 온라인 게임 등에 주로 사용한다.
