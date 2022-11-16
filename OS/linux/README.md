# Linux
### Linux의 사용 목적
* 오픈소스 소프트웨어 OS로 서버를 구축하기 위해 주로 사용
* Unix-Like-OS 
  * 개인용 컴퓨터에서 구동하는 유닉스같은 운영체제를 만들고 사용자가 스스로 커스터마이징하기 위함
### Linux 장점
1. **다중 사용자 및 다중 처리 시스템**
    * 멀티 유저
      * 하나의 시스템에 다수의 사용자들이 동시에 접근하여 사용 가능함
    * 멀티 태스킹
      * 각 접속자들은 다수의 작업을 CPU와 같은 공용자원을 나누어 다수의 응용 프로그램을 실행 가능함
2. **오픈소스 시스템**
3. **뛰어난 네트워크 환경**
    * 가장 널리 쓰이는 이더넷(Ethernet)을 포함하여 SLIP, PPP, ATM 등 다양한 네트워크 환경 지원
    * TCP/IP, IPX, AppleTalk 등 대부분의 네트워크 프로그램 지원
    * 리눅스에서 지원하는 네트워크 프로토콜
        1. 인터네트워크 패킷 익스체인지 (IPX: Internetwork Packet Exchange)
        2. 넷뷔 (NetBEUI)
        3. TCP/IP
        4. AppleTalk
4. **다양한 파일 시스템**
5. **뛰어난 이식성**
    * 약간의 어셈블리어와 대부분의 C언어로 작성되어 있음
    * C를 컴파일할 수 있으며, 어셈블리어 부분만 새롭게 만들고 C 부분을 다시 컴파일함으로써 쉽게 다른 시스템이나 환경 등에 이식하여 사용할 수 있음
6. **유연성 및 확장성**
    * 상업용 유닉스(UNIX)의 모든 특성을 가지고, 유닉스의 표준인 포직스(POSIX)를 준수하고 있음
    * 커널, 장치 드라이버, 라이브러리, 응용 프로그램, 개발도구 등 리눅스의 소스코드를 쉽게 접할 수 있음
7. **강력한 안정성과 보안성**
    * 커널 소스가 공개되어 있어 패쇄형 운영체제에 비해 보안상의 취약점이 쉽게 노출될 가능성이 있으나, 수많은 전문 프로그래머들이 상용 운영체제보다 빠르게 오류 수정과 보안 패치에 대응하여 안정성을 확보한 버전이 릴리즈되고 있음
8. **가격대 성능비**
    * PC급 서버에서도 엔터프라이즈(Enterprise) 서버와 유사한 성능을 발휘할 수 있음
9. **다양한 응용 프로그램 제공 및 다양한 배포판 존재**
### Linux 단점
1. **기술 지원의 부족**
    * 전 세계 흩어져 있는 개발자들이 일일이 기술지원을 하는 것은 어려움
2. **특정 하드웨어에 대한 자원 부족**
    * 이식성, 확장성은 뛰어나지만 여전히 특정 하드웨어에 대한 설치가 어렵고 모든 플랫폼에서 작동하는 만능 운영체제는 아님
3. **사용자의 숙련된 기술 요구**
    * 사용자들에게 명령어나 편집기와 같은 지식 요구