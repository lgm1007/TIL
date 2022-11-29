# GRANT (계정 권한 관리)
* 계정에게 권한을 부여하는 쿼리
```sql
GRANT 권한종류 ON 대상DB TO 계정명;
```
* 이후 변경된 내용 적용
```sql
FLUSH PRIVILEGES;
```
### GRANT 쿼리 설명
|권한|내용|
|---|---|
|CREATE, ALTER, DROP|테이블 생성, 변경, 삭제|
|SELECT, INSERT, UPDATE, DELETE|테이블의 레코드 조회, 입력, 수정, 삭제|
|RELOAD|권한 부여된 내용을 리로드|
|SHUTDOWN|서버 종료 작업 실행|
|ALL|모든 권한 허용|
|USAGE|권한 없이 계정만 사용|



