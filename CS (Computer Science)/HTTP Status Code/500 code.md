# 500 Code
* `500 Internal Server Error`
* 서버에 에러가 발생했다.
* 클라이언트가 모르는 5xx 계열의 응답 코드가 반환된 경우에도 클라이언트는 500과 동일하게 처리하도록 규정하고 있다.

### 원인
1. **코드 버그**
    * 서버 측 응용 프로그램의 버그나 오류로 인해 발생
    * 이는 코드의 잘못된 구현, 예상치 못한 예외 또는 잘못된 입력 처리와 관련될 수 있다.
2. **서버 구성 오류**
    * 잘못된 구성 파일 설정, 부적절한 권한, 필요한 리소스가 부족한 경우
3. **외부 서비스 연동**
    * 서버가 외부 서비스와 통신하는 경우, 해당 서비스의 문제로 인해 500 오류가 발생할 수 있다.
    * 외부 서비스의 일시적인 중단 또는 서버 간 통신 문제일 경우

### 해결 방안
1. **로그 확인**
    * 발생한 오류에 대한 자세한 정보 파악
    * 어떤 요청이 문제를 일으켰는지 파악 가능
2. **코드 리뷰**
    * 코드 리뷰로 잠재적인 버그나 오류를 찾아낸다.
    * 예외 처리를 강화하고, 잘못된 입력에 대한 처리를 개선해 예기치 못한 예외를 방지할 수 있다.
3. **서버 구성 확인**
    * 서버 구성 파일을 검토하여 문제가 있는 설정 수정
    * 권한 문제나 필요한 리소스 부족 등을 확인하여 수정할 수 있다.
4. **외부 서비스 연동 확인**
    * 외부 서비스와의 통신 문제인 경우, 외부 서비스의 상태를 확인하고 문제가 임시적인 것인지 영구적인 것인지 파악하여 대응
5. **사용자에게 적절한 오류 메시지 제공**
    * 사용자에게 문제가 서버 측에 있음을 알리고, 문제가 해결될 때까지 기다리거나 나중에 다시 시도할 것을 안내할 수 있다.

### 정리
* 500 상태 코드는 내부 서버 오류를 나타내는 코드로, 클라이언트의 요청을 처리하는 동안 서버 측에 예기치 못한 문제가 발생했음을 나타낸다.
* 서버 측의 문제로 클라이언트 요청을 성공적으로 처리할 수 없음을 의미한다.
