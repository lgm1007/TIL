# docker-compose
* docker-compose는 도커에서 사용하고자 하는 이미지를 바탕으로 컨테이너를 실행할 떄 필요한 명령어를 작성한 yml 파일
* 시스템 구축과 관련된 명령어를 하나의 파일에 기재해 명령어 한 번에 시스템 전체를 실행하고 종료, 폐기까지 한 번에 하도록 도움을 준다.

### docker-compose의 구조
#### 정의 파일 (compose file)
* 컨테이너나 볼륨을 어떠한 설정으로 만들지에 대한 항목 뿐 아니라 시스템에 대한 모든 정보가 기재된다.

##### 정의 파일 작성
* 예제 1) **db 컨테이너**
```yaml
db:
  container_name: spring-db
  image: mysql
  environment:
    MYSQL_DATABASE: spring_db
    MYSQL_USER: spring
    MYSQL_PASSWORD: spring_db_pw
    MYSQL_ROOT_PASSWORD: root_pw
  volumes:
    - ./db/data:/var/lib/mysql:rw
  ports:
    - "3306:3306"
  restart: always
```
* `container_name`: 컨테이너 이름 지정
* `image`: Docker hub에 올라가 있는 image 리스트에서 사용할 이미지 명과 태그
* `environment`: 컨테이너 내부에서 사용할 인자
* `volumes`: local 디렉토리와 컨테이너 경로 매칭
* `ports`: 외부에서 접속하는 포트를 Docker 내부 포트와 매칭
* `restart`: 컨테이너 오류처럼 종료되었을때 다시 시작 여부


* 예제 2) **app 컨테이너**
```yaml
app:
  container_name: spring-app
  image: openjdk:11-jdk
  ports:
    - "8080:8080"
  volumes:
    - ./app:/app
  working_dir: /app
  command: ["./gradlew", "bootrun"]
  depends_on:
    - db
  restart: always
```
* `working_dir`: 컨테이너가 실행되고 나서 `command` 명령어가 실행되는 위치
* `command`: Bash나 PowerShell과 같은 커맨드 명령어 실행
* `depends_on`: 컨테이너 실행 우선순위 지정, 하위로 작성된 컨테이너가 정상적으로 실행한 후에 해당 컨테이너 실행

##### 명령어
* **up 명령어**
    * `docker run` 커맨드와 비슷
    * 정의 파일에 작성된 내용대로 도커 이미지를 내려받고 컨테이너 생성 및 실행하는 명령어
    * 정의 파일에 네트워크나 볼륨에 대한 정의도 기재할 수 있어 주변 환경을 한꺼번에 생성 가능하다.
    * up 명령어 파라미터
      * `-build`: docker-compose up으로 컨테이너 생성 시, 이미지를 새롭게 다시 빌드하는 것을 의미한다.
      * `-d`: 백그라운드 환경에서 컨테이너를 실행하는 것을 의미한다.
* **down 명령어**
    * 컨테이너와 네트워크 정지 및 삭제
    * **볼륨과 이미지는 삭제하지 않는다.**
        * 만약 컨테이너와 네트워크를 삭제하지 않고 정지만 하고 싶다면 **stop 명령어**를 사용한다.


* 예제) docker-compose 터미널 명령어
```
# docker-compose를 실행하여 컨테이너 생성/실행
> docker-compose up -build -d

# docker-compose 실행 종료 및 컨테이너/네트워크 삭제
> docker-compose down
```

### docker-compose 설치
1. linux
   * 리눅스 환경에서는 설치 명령어 또는 깃허브에서 직접 내려받아 설치할 수 있다.
```
$ sudo curl -L <https://github.com/docker/compose/releases/download/{설치버전}/docker-compose-`uname> -s`-`uname -m` -o /usr/local/bin/docker-compose

$ sudo chmod +x /usr/local/bin/docker-compose
```


2. Windows / MacOS
   * 윈도우와 MacOS에서는 Docker Toolbox나 Docker for Windows, Docker for Mac을 설치하면 도커 엔진과 함께 설치된다.

  
3. 설치완료 후 버전 확인
```
$ docker-compose -v
Docker Compose version v2.15.1
```
