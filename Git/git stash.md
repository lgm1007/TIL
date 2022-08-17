# git stash
* 커밋하지 않고 변경사항을 임시 저장하는 방법
* stash는 스택처럼 추가/제거됨
### 현재까지 작업하던 내역 임시 저장
```
/** git 저장소에서 관리하고 있는 파일들을 대상으로 실행, 스테이지에 있는 내용과 아직 스테이지에 들어가지 않은 변경사항 모두 저장 
  * git stash = git stash save */
  $ git stash
  $ git stash save
```
### stash 목록 확인
```
  $ git stash list
```
### stash 불러오기
```
1. 가장 최근의 stash 불러오기 
/** --index 옵션을 붙여야 스테이지 상태로 불러온다 */ 
  $ git stash apply --index

2. 특정 stash 불러오기
  $ git stash apply [stash이름]
```
### stash 삭제
```
1. 가장 최근의 stash 삭제
  $ git stash drop

2. 특정 stash 삭제
  $ git stash drop [stash이름]
```