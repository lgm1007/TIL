# Git 저장소 Mirror
* Git 저장소를 다른 원격 저장소로 **커밋 로그를 포함**하여 온전히 복제하는 방법을 알아본다.

### git mirror
> git mirror: 커밋 로그를 잃지 않고, 레포지토리를 복제할 때 사용하는 명령어

### 사용법
1. 복사할 레포지토리를 local에서 미러링한다.
   * `> git clone --mirror <레포지토리 URL>`
2. 미러링이 되면 생기는 `.git` 폴더로 이동한다.
   * `cd 레포지토리.git`
3. 복사를 진행할 새 레포지토리 URL을 입력하여 remote 한다.
   * `git remote set-url --push origin <새 레포지토리 URL>`
4. 패치 후 미러 푸쉬를 진행한다.
   * `git fetch -p origin`
   * `git push --mirror`

### 이슈트래킹
* `git push --mirror` 하는 경우, `remote rejected` 오류가 뜨는 경우가 있으나, 새 저장소에는 정상적으로 미러되는 경우
  * 아래 방법으로 진행하면 오류가 발생하지 않음
```
> git clone --bare <레포지토리 URL>
> cd 레포지토리.git
> git remote set-url --push origin <새 레포지토리 URL>
> git push --mirror
```
