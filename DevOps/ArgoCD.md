# ArgoCD
### GitOps
* DevOps의 실천 방법 중 하나로, 애플리케이션의 모든 배포와 운영에 관려된 모든 요소들을 **Git에서 관리 (Operation)** 한다는 의미
* GitOps는 Git Pull 요청을 사용하여 인프라 프로비저닝 및 배포를 자동으로 관리함
* Git 레포지토리에는 전체 시스템 상태가 포함되어 있어 시스템 상태의 변화 추이를 확인 및 감시할 수 있음
* **GitOps의 원칙**
  1. 모든 시스템은 선언적으로 선언되어야 한다.
  2. 시스템의 상태는 Git의 버전을 따라간다.
  3. 승인된 변화는 자동으로 시스템에 적용된다.
  4. 배포에 실패하면 이를 사용자에게 경고해야 한다.
### ArgoCD
* GitOps 방식으로 관리되는 **Manifest(yaml) 파일의 변경사항을 감시**하며, 현재 배포된 환경의 상태와 **Github Manifest 파일에 정의된 상태를 동일하게 유지**하는 역할 수행
### ArgoCD의 주요 기능
1. GitOps
	* 모든 애플리케이션 배포 작업은 Git 레파지토리에서 커밋으로 수행한다.
2. CI/CD 통합
	* 다양한 CI/CD 도구와 통합될 수 있다.
3. Declarative 구성
	* Kubernetes 애플리케이션 배포를 위해 yaml 파일 사용한다.
4. 롤백 지원
	* 이전 배포로 롤백하는 것이 가능하다.
5. 암호화 지원
	* 데이터 보호를 위한 암호화 지원도 제공된다.
6. 관리 UI
### ArgoCD 애플리케이션 배포
1. Git 저장소에 Manifest 파일 업로드
	* ArgoCD는 GitOps 패턴을 따르기 때문에 Manifest를 저장할 Git 저장소가 필요
    * ArgoCD는 이 저장소에서 변경 사항을 감시하여 새로운 버전의 Manifest를 가져와 Kubernetes 클러스터에 배포함
2. ArgoCD가 Git 저장소에서 Manifest 가져옴
3. ArgoCD가 Kubernetes 클러스터에 애플리케이션 배포
	* ArgoCD는 배포에 필요한 Kubernetes 오브젝트를 생성하고 관리함
    * Kubernetes 클러스터와의 연결을 설정하고 ArgoCD를 설치하면 클러스터에 대한 권한 설정
    * ArgoCD는 다양한 배포 전략을 지원함
      * Canary, Blue/Green, Rollout 등
      * 이러한 전략은 Manifest에 정의되며, ArgoCD가 이를 기반으로 배포 수행

