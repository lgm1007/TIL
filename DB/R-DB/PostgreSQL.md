# PostgreSQL
* 오픈 소스 객체-관계형 데이터베이스 시스템 (ORDBMS)
* 다른 관계형 데이터베이스 시스템과 달리 **연산자, 복합 자료형, 집계 함수, 자료형 변환자, 확장 기능** 등 다양한 데이터베이스 객체를 **사용자가 임의로 만들 수 있는 기능** 제공
  * **새로운 하나의 프로그래밍 언어**처럼 구현 가능
### PostgreSQL 구조
* **클라이언트/서버 모델** 사용
  * 서버는 데이터베이스 파일을 관리하며, 클라이언트 애플리케이션으로부터 들어오는 연결을 수용하고, 클라이언트 대신하여 데이터베이스 액션 수행
  * 서버는 다중 클라이언트 연결을 처리할 수 있는데, 클라이언트 연결이 오면 각 커넥션에 대해 새로운 프로세스를 fork함
  * 클라이언트는 기존 서버와의 간섭 없이 새로 생성된 서버 프로세스와 통신하게 됨
### PostgreSQL 특징
* **Portable**
  * 지원하는 플랫폼의 종류로는 Windows, Linux, MAC OS/X, Unix 등 다양한 플랫폼 지원
* **Reliable**
  * 트랜잭션 속성인 ACID에 대한 구현 및 MVCC
  * 로우 레벨 Locking 구현
* **Scalable**
  * 멀티 버전에 대한 사용 가능
  * 대용량 데이터 처리를 위한 Table Partitioning과 Tables Space 기능 구현 가능
* **Secure**
  * DB 보안은 데이터 암호화, 접근 제어 및 감시 3가지로 구성
  * 호스트 기반의 접근 제어, Object-Level 권한, SSL 통신을 통한 클라이언트와 네트워크 구간의 전송 데이터 암호화 등 지원
* **Recovery & Availability**
  * Streaming Replication을 기본으로, 동기식/비동기식 Hot Standby 서버 구축 가능
  * WAL Log 아카이빙 및 Hot Back up을 통해 Point in time recovery 가능
* **Advanced**
  * pg_upgrade를 통해 업그레이드를 할 수 있으며, 웹 또는 C/S 기반의 GUI 관리 도구를 제공하여 모니터링 및 관리, 튜닝 가능
  * 사용자 정의 Procedural로 Perl, Java, PHP 등의 스크립트 언어 지원 가능

