# 502 Code
* `502 Bad Gateway`
* 게이트웨이 또는 프록시 역할을 하는 서버가 그 뒷 단의 서버로부터 잘못된 응답을 받았다.

### 원인
1. **서버 다운 또는 부팅 중인 경우**
    * 클라이언트가 요청한 서버가 다운되거나 부팅 중인 경우 발생 가능
2. **게이트웨이나 프록시 구성 오류**
    * 게이트웨이나 프록시 서버가 클라이언트의 요청을 다른 서버로 전달하는 동안 구성 오류가 있는 경우 발생 가능
3. **서버 응답 시간 초과**
    * 게이트웨이 서버가 대상 서버에서 응답을 받지 못하거나 응답 시간이 지나치게 길어져 클라이언트에게 적절한 응답을 제공할 수 없는 경우 발생 가능

### 해결 방안
1. **서버 확인**
    * 클라이언트가 요청한 서버 상태 확인
    * 서버가 정상적으로 작동하는지 필요하면 다시 시작하거나 재부팅
2. **게이트웨이 또는 프록시 구성 확인**
    * 프록시 서버나 게이트웨이 서버의 설정이 올바른지 확인
    * 필요하면 구성을 수정하고 다시 시도
3. **응답 시간 조정**
    * 대상 서버가 응답하는 데 오랜 시간이 걸린다면 응답 시간을 조정하거나 대상 서버의 부하를 감소시켜야 한다.
4. **로그 확인**
    * 서버 로그를 확인하여 무엇이 잘못되었는지 파악하고 문제를 해결할 수 있는 정보 확인

### 정리
* **502 Bad Gateway 와 504 Gateway Timeout 과의 차이**
  * 502 코드는 게이트웨이나 프록시 서버에서 발생하는 오류
  * 504 코드는 게이트웨이나 대상 서버에서 제때 응답을 받지 못한 경우에 발생하는 오류
* **서버 간 통신 이슈**
  * 502 코드는 주로 서버 간 통신 문제로 인해 발생하므로 서버 간 통신을 검토하고 보안 그룹 등이 올바르게 구성되어 있는지 확인해야 한다.
