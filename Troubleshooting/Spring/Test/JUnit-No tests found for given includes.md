# [JUnit Error] No Tests found for given includes: [~~]
### 에러 상황
테스트 코드 실행하는데 아래와 같은 에러가 발생하는 문제

```
Execution failed for task: ':test'.
> No tests found for given includes: [테스트코드경로.테스트코드 메서드명](--tests filter)

* Try:
> Run with --stacktrace option to get the stack tree.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.
```

### 에러 발생 원인
1. JUnit4 에서 JUnit5 로 업데이트되면서 org.Junit.Test 가 아닌 org.Junit.Test.api.Test 를 사용해야 함
2. JUnit4 에서 JUnit5 로 넘어오며 build.gradle 에 아래 테스트 설정을 추가해야 함
```
test {
    useJUnitPlatform()
}
```

### 추가적인 체크 사항
* IntelliJ Settings > Build, Execution, Deployment > Build Tools > Gradle 설정에서 Run tests using: 옵션을 다르게 변경해보기
* build.gralde에서 `testImplementation` 이 잘못 설정되어 있는지 확인
