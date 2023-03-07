# REST
* `REpresentational State Transfer`의 약자
* 자원(source)의 표현(repersentation)에 의한 상태 전달을 의미
  * 자원: 해당 소프트웨어가 관리하는 모든 것 (문서, 이미지, 데이터 등)
  * 표현: 자원을 표현하기 위한 이름
  * 상태 전달: 데이터가 요청되는 시점에 자원의 상태를 전달 (JSON 또는 XML)
* 네트워크 상의 Client와 Server 사이의 통신 방식 중 하나
### 개념 설명
* 어떠한 자원에 대해 CRUD(Create, Read, Update, Delete) 연산을 수행하기 위해 URI(Resource)로 GET, POST 등의 방식을 이용하여 요청을 보내며, 요청을 위한 자원은 특정한 형태(Representation of Resource)로 표현
	* **URI와 URL의 차이**
      * URI: Uniform Resource Identifier, 인터넷 상의 자원을 식별하기 위한 문자열의 구성
      * URL: Uniform Resource Location, 인터넷 상의 자원의 위치
        * URI는 URL을 포함
### 구성 요소
1. **자원(Resource) - URI**
   * 모든 자원에는 고유한 ID가 존재하고 이 자원은 Server에 존재
   * 자원을 구별하는 ID는 HTTP URI
   * Client는 URI를 이용해 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청
2. **행위(Verb) - Method**
   * HTTP 프로토콜의 Method(GET, POST, PUT, PATCH, DELETE) 사용
     * GET: Read, 정보 요청, URI가 가진 정보를 검색하기 위해 요청
     * POST: Create, 정보 입력, Client가 Server로 정보를 전달
     * PUT: Update, 정보 업데이트, 데이터 전체를 바꿀 때
     * PATCH: Update, 정보 업데이트, 데이터 일부를 바꿀 때
     * DELETE: Delete, 정보 삭제, 안정성 문제로 서버에서 대부분 비활성화
3. **표현(Representation of Resource)**
   * Client와 Server가 데이터를 주고받는 형태, JSON, XML, TEXT, RSS
   * JSON, XML이 일반적
### 특징
1. **Server-Client (서버-클라이언트 구조)**
   * 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client
2. **Stateless (무상태)**
   * HTTP 프로토콜은 Stateless Protocol이므로 REST 또한 무상태성
3. **Cacheable (캐시 처리 기능)**
   * HTTP 프로토콜의 캐시 처리 기능을 활용해 리소스를 캐시하고 이를 이용하여 성능 개선
4. **Layered System (계층 구조)**
   * Client와 Server 사이에 중간 계층(Proxy, Gateway)을 두어, 시스템의 확장성을 향상시키고 보안성을 유지하며 서비스를 제공
   * 중간 계층을 이용하면 Client와 Server가 직접 연결하지 않아도 되므로, 시스템의 확장성을 향상시킬 수 있음
5. **Uniform Interface (인터페이스 일관성)**
   * 리소스를 식별하고 조작하기 위해 통일된 인터페이스 제공
   * 4가지 제약 조건으로 구성
     1. 리소스 식별(Resource Identification): 모든 리소스는 고유한 식별자를 가지고 있어야 함
     2. 표현을 통한 리소스 조작(Manipulation of Resources Through Representations): Client는 Server에게 리소스를 조작하기 위한 메시지 전송
     3. 자체 표현 메시지(Self-Descriptive Messages): 메시지는 메시지 자체로 이해가 가능한 내용을 지니고 있어야 함
     4. 애플리케이션 상태의 하이퍼미디어 엔진(Hypermedia as the Engine of Application State): 애플리케이션의 리소스의 상태 변경, 생성은 하이퍼미디어를 이용
6. **Self-Descriptiveness (자체 표현)**
   * 메시지 자체가 필요한 정보를 포함하여, 메시지만으로도 요청의 의미와 리소스의 타입, 상태 등을 이해할 수 있도록 함

# REST API

