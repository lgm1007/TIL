# [Maven] Blocked mirror for repositories
### 에러상황
* Maven 3.8.1 버전부터는 http 에 대한 외부 접근을 막는 것으로 변경
* Maven 3.8.1 버전에서 pom.xml 내 `<repository>`에 http:// 로 시작하는 설정이 존재할 때 발생하는 문제

### 해결방법
* `settings.xml` 파일 편집
  * `settings.xml` 파일 경로는 InteliJ의 경우 settings > Build, Execution, Deployment > Build Tools > Maven 탭에서 User settings file 경로를 확인하여 해당 경로의 `settings.xml` 파일 편집

```xml
<mirrors>
    <mirror>
        <id>maven-default-http-blocker</id>
        <mirrorOf>external:dont-match-anything-mate:*</mirrorOf>
        <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
        <url>http://0.0.0.0/</url>
    </mirror>
</mirrors>
```
* 위 내용을 추가한다.
