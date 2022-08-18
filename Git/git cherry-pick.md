# git cherry-pick
* 다른 브랜치에 있는 커밋을 선택하여 현재 브랜치에 적용시키는 방법
### 특정 커밋을 현재 브랜치에 적용
```
$ git cherry-pick [커밋ID]

/* 또는 */

$ git cherry-pick [커밋ID 1] [커밋ID 2]
```
### 연속적인 커밋 적용
```
$ git cherry-pick [시작 커밋ID]..[종료 커밋ID]
```
### 충돌 해결 방법
1. 충돌 해결 후 cherry-pick을 계속 진행햘 때
    1. 충돌난 코드 수정
    2. `git add [충돌난 파일 경로]`로 충돌 해결한 코드 추가
    3. `git cherry-pick --continue`로 cherry-pick 다시 진행
2. cherry-pick 중단할 때
   1. `git cherry-pick --abort`로 cherry-pick 중단하고 이전 상태로 돌리기
##### 주의. cherry-pick는 같은 내용을 가진 커밋을 여러 개 생성하는 개념으로, 꼭 사용해야 하는 경우에만 사용한다.