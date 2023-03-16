# Docker 빌드 및 배포
* 도커 이미지 빌드 및 배포 관련하여 몰랐던 부분을 정리해보는 문서
### 도커 이미지를 빌드하기 위해 필요한 것들은?
1. **Dockerfile**
	* 이미지를 빌드하기 위한 설정 파일
    * [Dockerfile](./Dockerfile.md)
2. **Docker 클라이언트**
	* Docker CLI (Command Line Interface)를 사용하여 Dockerfile을 빌드하고, 이미지를 생성함
3. **Docker 데몬**
	* Docker CLI가 커맨드를 실행할 때 사용하는 Docker 엔진
4. **Docker 레지스트리**
	* Docker 이미지를 저장하고 관리하는 원격 저장소
    * 대표적으로 Docker Hub, ECR, Google Container Registry 등이 있음
* Docker 클라이언트와 Docker 데몬은 OS에 따라 아래와 같이 설치 가능
  * Docker Desktop (Windows/MacOS): https://www.docker.com/products/docker-desktop
  * Docker Engine (Linux): https://docs.docker.com/engine/install/
### 도커 이미지 빌드하기
* (Windows OS에 Docker Desktop이 설치되어 있는 환경)
* PowerShell 또는 명령 프롬프트 등의 터미널에서 명령 실행
  * Docker CLI 가 터미널 환경에서 실행되기 때문
```shell
docker build -t <이미지 이름>:<태그> <Dockerfile 경로>
```
* 빌드된 이미지는 `docker images` 명령어로 확인
* 빌드한 이미지의 데이터는 Docker Desktop의 VM (가상머신)에 저장됨
  * Docker Desktop은 Windows OS와 Linux 컨테이너를 실행하기 위해 Hyper-V 기술을 사용해 가상머신을 실행함
### 도커 컨테이너 실행하기
* `docker run` 명령어로 컨테이너를 실행하면서 로컬 머신과 컨테이너 간 포트 매핑 설정
```shell
docker run -p 8888:8080 my-image:latest
```
* 로컬 머신의 8888번 포트와 컨테이너 내부의 8080번 포트 매핑
### 컨테이너 테스트
1. 로컬 환경에서 컨테이너 실행 및 테스트
	* 도커 이미지 빌드, 로컬 환경에서 컨테이너 실행
    * 실행한 컨테이너에서 애플리케이션 실행, 동작 테스트
2. 클라우드 환경에서 컨테이너 실행 및 테스트
	* 클라우드 환경에서 이미지 실행, 애플리케이션 테스트
3. 배포
	* 첫 번째 배포 방법
      * Docker Hub나 Docker Registry와 같은 Docker 이미지 저장소 이용
      * 저장소에 이미지 업로드 후 필요한 곳에서 다운로드하여 사용
      * 이미지 공유 및 배포가 간편하다는 장점
    * 두 번째 방법
      * 이미지를 직접 복사하여 사용하는 방법
      * 보안적인 이유로 많이 사용하는 방법은 아님
    * 세 번째 방법
      * Kubernetes와 같은 컨테이너 오케스트레이션 툴을 이용한 방법
      * [Kubernetes](./Kubernetes.md)
      * 복잡한 구성이 필요하지만, 확장성과 안정성 등에서 이점이 있음

