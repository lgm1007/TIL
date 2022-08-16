# git reset
* git에서 이전 커밋으로 되돌아가는 대표적인 방법
### git add 취소 (파일 상태 Unstage로 변경하기)
* 실수로 git에 추가하지 않을 파일을 add한 경우
  * [File명]이 없으면 git add된 파일 전체 취소
```
$ git reset HEAD [File명]
```
### git commit 취소
* 너무 일찍 커밋한 경우 또는 이전 커밋으로 되돌아갈 경우
```
1. commit 취소하고 파일들은 staged 상태로 워킹 디렉토리에 보존
  $ get reset --soft HEAD^

2. commit 취소하고 파일들은 unstaged 상태로 워킹 디렉토리에 보존
  /* 기본 옵션 */
  $ git reset --mixed HEAD^
  /* 위와 동일한 동작 */
  $ git reset HEAD^
  /* 마지막 2개의 commit 취소 */
  $ git reset HEAD~2

3. commit 취소하고 파일들은 unstaged 상태로 워킹 디렉토리에서 삭제
  $ git reset --hard HEAD^
  /* 원하는 시점으로 워킹 디렉토리 되돌리기 */
  $ git reset --hard [commit id]
```
### 원격 저장소 상태로 되돌리기
```
$ git reset --hard origin/master
```