# unable to update local ref
### 문제 상황
* git pull을 수행할 때 다음과 같은 에러가 발생할 때가 있다.

```
error: 'refs/remotes/origin/xxx' exists; cannot create 'refs/remotes/origin/xxx/xxx-branch'
From https://github.com/... git repository 경로
 ! [new branch]      xxx-branch -> origin/xxx/xxx-branch  (unable to update local ref)
```

* 불필요하게 브랜치 생성이 꼬인 경우 발생하는 에러로 다음 명령어로 꼬인 상태를 해결할 수 있다.

```
$ git gc --prune=now
$ git remote prune origin
```
