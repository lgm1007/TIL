# Harbor
* 도커 레지스트리 서비스 중 하나
* 오픈소스로 제공되는 프로젝트로 Docker Hub 처럼 컨테이너 이미지를 저장할 수 있는 저장소
### Harbor의 특징
* 보안
  * 사용자 인증 및 권한 부여, 이미지 스캐닝, 서명된 이미지 및 TLS 암호화를 포함한 다양한 보안 기능 제공
* 정책 관리
  * 사용자 지정 정책을 설정하여 이미지 빌드, 업로드 및 다운로드를 제어할 수 있음
* CI/CD 통합
  * CI/CD 파이프라인에서 이미지 레지스트리를 자동으로 업데이트할 수 있도록 지원
* 이미지 복제
  * 여러 지역 또는 클라우드에 걸쳐 이미지를 자동으로 복제할 수 있음
* 대규모 확장성
  * 수천 개의 노드에서 대규모 이미지 레지스트리를 지원할 수 있음
### Harbor 장점
1. 보안성
	* 이미지에 대한 보안성 강화를 위해 이미지 스캐닝, RBAC(롤 기반 엑세스 제어), 이미지 서명 등의 보안 기능 제공
2. 프로젝트/레파지토리 관리
	* 프로젝트 및 레파지토리를 관리하기 위한 기능 제공
    * 사용자는 이미지를 논리적으로 구성하고 관리 가능
3. 확장성
	* 쉽게 확장 가능한 아키텍처 제공
    * 많은 수의 사용자 및 이미지 지원
    * 다른 컨테이너 오케스트레이션 시스템과 통합 가능
4. 오픈소스
	* 무료로 사용 가능하며 사용자는 소스 코드를 검사하고 수정할 수 있음
### Harbor 단점
1. 설치 및 구성
	* 설치 및 구성이 복잡
    * 러닝 커브
2. 성능
	* 높은 성능을 제공하지만, 더 많은 리소스를 사용하여 성능을 유지해야 하기 때문에 비용과 관련된 문제가 야기될 수 있음
3. 기능 부족
	* 다른 도커 레지스트리와 비교하면 몇 가지 기능이 누락되어 있음
    * 사용자가 필요로 하는 기능을 찾지 못할 수도 있음
4. 문서화
	* 문서가 다소 미흡함
### 로컬에서 Harbor 이미지 push/pull
* 필요 조건
  * Docker 클라이언트가 설치되어 있어야 함 [[Docker 빌드 및 배포]](./Docker%20빌드%20및%20배포.md)
  * Harbor 접속 이메일과 CLI secret 값
    * Harbor 로그인하고 User Profile에서 CLI secret 값 얻을 수 있음
    * <img width="400" alt="USERPROFILE" src="https://user-images.githubusercontent.com/80727821/226561638-5bd1d36d-3a83-4971-a150-740272cccdee.png">
* **이미지 Push 하기**
  * docker login 진행 (Push/Pull 공통 사항)
    * ```cmd
      docker login <Harbor 주소>
      ```
      * id: Harbor 유저 이메일 주소
      * pwd: CLI secret 값
  * Push 하려고 하는 이미지 태그 설정
    * ```cmd
       docker tag SOURCE_IMAGE[:TAG] <Harbor 주소>/PROJECT명/REPOSITORY명[:TAG]
      ```
  * Push 하기
    * 해당 Harbor의 프로젝트에 push 권한이 있어야 진행 가능
    * ```cmd
       docker push <Harbor 주소>/PROJECT명/REPOSITORY명[:TAG]
      ```
* **이미지 Pull 하기**
  * docker login 진행 후 pull 진행
    * 해당 Harbor의 프로젝트에 pull 권한이 있어야 진행 가능
    * ```cmd
       docker pull <Harbor 주소>/PROJECT명/IMAGE명[:TAG]
      ```

