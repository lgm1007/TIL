# Github Actions
* 코드 저장소로 유명한 Github에서 제공하는 CI(Continuous Integration, 지속 통합)와 CD(Continuous Deployment, 지속 배포)를 위한 서비스이다.
* Github에서 코드를 관리하고 있는 소프트웨어 프로젝트에서 바로 사용이 가능하기 때문에 타 CI/CD 서비스 대비 진입장벽이 낮은 편이다.

### 기대 효과
* 자동으로 코드 저장소에서 어떠한 이벤트가 발생했을 때 특정 작업이 일어나게 해주거나 어떤 작업을 반복해서 실행시킬 수도 있다.
* 동작의 한 예로, 코드 저장소에 *Pull Request를 생성하게 되면 Github Actions를 통해 해당 코드의 변경 부분에 문제가 없는지 각종 검사를 진행할 수 있으며 어떤 새로운 코드가 메인 브랜치에 Push 되면 Github Actions를 통해 소프트웨어를 빌드하고 상용 서버에 배포할 수 있다.
  * Pull Request : Github 저장소에 있는 브랜치에 작업한 사항들을 반영하기 전에 다른 사람들과 논의, 코드 리뷰할 수 있도록 요청하는 기능
* 그날 하루에 대한 통계 데이터 또한 수집할 수 있다.
### 구성
* Workflows
* Jobs
* Steps
* Actions

#### Workflows
* Github Actions에서 가장 상위 개념으로 자동화해 놓은 작업 과정이다.
* 코드 저장소 내에서 .github/workflows 디렉토리 아래 위치한 YAML 파일로 설정하며 하나의 코드 저장소에는 여러 개의 Workflows (여러 개의 YAML 파일)를 생성할 수 있다.  
* 예시 1. 코드 저장소의 메인 브랜치에 Push 이벤트가 발생할 때마다 Workflow를 실행하기 위한 설정  
.github/workflows/example.yml
``` yaml
on:
push:
branches:
- main
jobs:
# (생략)...
```

* 예시 2. 매일 자정에 Workflow를 실행하기 위한 설정

.github/workflows/example.yml
```yaml
on:
schedule:
- cron: "0 0 * * *"
jobs:
# (생략)...
```

#### Jobs
* Github Actions에서의 Job은 독립된 가상 머신 또는 컨테이너(Container)에서 돌아가는 하나의 처리 단위를 의미한다.
* 하나의 Workflow는 여러 개의 Jobs 로 구성되어 있으며 적어도 하나의 Job은 있어야 한다.
* 모든 Jobs들은 기본적으로 동시에 실행되며 필요 시 Jobs 간에 의존 관계를 설정하여 작업이 실행되는 순서를 제어할 수도 있다.
* Jobs는 YAML 설정파일 내의 jobs 속성을 통해 사용하며 작업 식별자(ID)와 작업 세부 내용 간의 맵핑(mapping) 형태로 명시한다.
* Jobs의 세부 내용으로는 여러 가지 내용을 명시할 수 있으며 필수로 들어가야 하는 runs-on 속성을 통해 Linux나 Windows와 같은 실행 환경을 지정해줘야 한다.
* 예시 3. Ubuntu 최신 실행 환경에서 해당 작업을 실행하고자 하는 경우

.github/workflows/example.yml
```yaml
# (생략) ...

jobs:
job1:
runs-on: ubuntu-latest
steps:
# (생략) ...
```

#### Steps
* 하나의 Job에 대해 여러 단계의 명령을 순차적으로 실행해야 하는 경우 Steps 속성을 통해 작업 순서를 정의한다.
* 각 Job이 하나 이상의 Step으로 모델링된다.
* Steps는 단순한 커맨드(Command)나 스크립트(Script)가 될 수도 있고 액션(Action)이라는 좀 더 복잡한 명령일 수도 있다.  
(커맨드나 스크립트를 사용할 때는 run 속성을 사용하며 액션을 사용할 때는 uses 속성을 사용한다.)
* 예시 4. 자바스크립트 프로젝트에서 테스트를 돌리기 위한 단계
	- 1단계 : 코드 저장소에서 코드를 작업 실행 환경으로 내려받는다.
	- 2단계 : 패키지를 설치한다.
	- 3단계 : 테스트 스크립트를 실행한다.
```yaml
# (생략) ...

jobs:
test:
runs-on: ubuntu-latest
steps:
- uses: actions/checkout@v3
- run: npm install
- run: npm test
```

#### Actions
* Github Actions의 꽃이라고 볼 수 있는 개념으로, Github Actions에서 빈번하게 필요한 반복 단계를 재사용하기 용이하게끔 제공되는 일종의 작업 공유 메커니즘이다.
* Actions은 하나의 코드 저장소 범위 내에서 여러 Workflow 간에서 공유할 수 있을 뿐 아니라, 공개(Pubilc) 코드 저장소를 통해 Action을 공유하면 Github 상의 모든 코드 저장소에서 사용이 가능해진다.
* Actions의 한 예로, 위 (예시 4.)에서도 사용한 actions/checkout 을 볼 수 있다.  
(코드 저장소로부터 코드를 작업 실행 환경으로 내려받는 작업을 하는 Action)
* Github MarketPlace에서 수 많은 벤더(Vendor)들이 공개한 다양한 Actions들을 찾아볼 수 있어, 여러 사용자와 벤더들이 Github Actions로 몰려드는 선순환이 발생하고 있다.