# Nginx
### Nginx 란?
* **경량 웹 서버**
* 클라이언트로부터 요청을 받을 때 요청에 맞는 정적 파일을 응답해주는 HTTP Web Server
* Reverse Proxy Server로 활용하여 WAS 서버의 부하를 줄일 수 있는 로드 밸런서로 활용되기도 함
### Nginx의 흐름
* **Event-Driven 구조**로 동작하기 때문에 한 개 또는 고정된 프로세스만 생성하여 사용
  * 비동기 방식으로 요청들을 동시 처리 가능
  * 새로운 요청이 들어오더라도 새로운 프로세스와 스레드를 생성하지 않기 때문에 **프로세스와 스레드 생성 비용이 존재하지 않으며**, **작은 자원으로도 효율적인 운용 가능**
  * 단일 서버에서도 많은 연결을 처리할 수 있음
### Nginx의 구조
* **하나의 Master Process**와 **다수의 Worker Process**로 구성되어 실행됨
  * Master Process: 설정파일을 읽고, 유효성 검사 및 Worker Process를 관리
  * 모든 요청은 Worker Process에서 처리
  * Worker Process의 개수는 설정 파일에서 정의되며, 정의된 프로세스 개수와 사용 가능한 CPU 코어에 맞게 자동으로 조정됨