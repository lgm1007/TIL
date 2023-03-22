# Amazon EKS
### Amazon EKS 개요
* Amazon EKS(Elastic Kubernetes Service)는 자체 Kubernetes 제어 영역이나 작업자 노드를 설치할 필요 없이 AWS에서 Kubernetes를 쉽게 실행할 수 있도록 지원하는 관리형 서비스이다.
* 다양한 AWS 서비스와 통합되어 애플리케이션에 대해서 확장성과 보안을 제공한다.
### Amazon EKS 특징
1. 관리형 서비스
	* 사용자는 클러스터 노드의 프로비저닝, 클러스터 업그레이드, 보안 패치, 고가용성 및 확장성 등의 작업을 수동으로 수행하지 않아도 됨
    * EKS에서 자동으로 처리하기 때문에 사용자는 애플리케이션 개발에 집중할 수 있음
2. Kubernetes 호환성
	* Kubernetes API를 완전히 지원하므로 모든 기존의 Kubernetes 리소스 정의 파일과 호환됨
3. 보안
	* 클러스터를 보호하기 위한 여러 가지 보안 기능 제공
    * 클러스터의 마스터 노드는 VPC 내 배치되어 있으며, 클러스터 네트워크 트래픽은 보안 그룹 및 네트워크 ACL을 사용하여 제한함
4. 확장성
	* EKS는 쉽게 확장 가능하며, 수천 개의 노드를 지원함
    * 사용자는 필요에 따라 클러스터 노드 수를 조정할 수 있으며 EKS에서 이를 자동으로 처리함
### Amazon EKS 장점
1. 쿠버네티스 전문 지식이 없어도 사용 가능
2. 관리형 서비스이므로 클러스터 관리, 보안 등의 문제를 대신 처리해 줌
3. Kubernetes API 호환성을 지원하여, 기존 Kubernetes 리소스 정의 파일과 호환됨
4. 쉽게 확장 가능
5. 안정적인 서비스
### Amazon EKS 단점
1. 가격이 비교적 높음
2. AWS 외부에 있는 클러스터와의 연결이 AWS 내부와 비교하여 복잡할 수 있음
### Amazon EKS 사용하기
1. EKS 클러스터 생성
	* AWS Management Console, AWS CLI 또는 AWS SDK를 사용하여 EKS 클러스터를 생성할 수 있음
    * EKS 클러스터는 하나 이상의 EC2 인스턴스에서 실행됨
    * EKS 클러스터를 생성하기 전에 IAM 역할 생성
2. Worker 노드 추가
	* EKS 클러스터가 생성되면 Kubernetes 워커 노드를 생성해야 함
    * Amazon EKS optimized AMI를 사용하여 EC2 인스턴스 시작할 수 있음
3. 애플리케이션 배포
	* Kubernetes 클러스터에서 애플리케이션을 배포하려면 Kubernetes 매니페스트 파일을 사용하여 애플리케이션의 구성을 정의해야 함
    * 매니페스트 파일은 클러스터에 배포할 Kubernetes 오브젝트 (서비스, 레플리카 셋, 파드 등) 를 정의함
4. 클러스터 관리
	* EKS API를 사용하여 클러스터 구성을 업데이트 하거나, 클러스터의 상태를 모니터링 할 수 있음
### etc) Kubernetes 설명
* [Kubernetes.md](./Kubernetes.md)

