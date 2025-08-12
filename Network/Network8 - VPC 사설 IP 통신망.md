## VPC 사설망 외부 통신
만약 외부 인터넷을 차단하는 private 서브넷에 RDS 인스턴스를 두고 사용 중이었는데, RDS가 외부 인터넷을 통해 업데이트를 해야 할 일이 생긴다면 어떻게 해야할까?  
공인 IP를 생성해서 private 서브넷에도 Internet Gateway를 통해 연결해줘야 할까? 그러면 이는 public 서브넷이지 더 이상 private 서브넷이 아니게 된다.

바로 이런 경우에 사용하는 것이 **NAT Gateway**와 **Bastion Host** 기술이다.  
이 둘은 사설망을 유지시켜주면서 외부와 조건적으로 통신이 가능하게 해준다.

## NAT Gateway
**네트워크 주소 변환** (**NAT**: Network Address Translation)은 IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터로 네트워크 트래픽을 주고 받는 기술이다.  
대표적으로 **사설 IP를 외부 인터넷과 통신하기 위해** 사용한다.

NAT을 사용하는 이유는 대개 사설 네트워크에 속한 여러 개의 호스트가 **하나의 공인 IP 주소를 사용**하여 인터넷에 접속하기 위함이다.  
인터넷 접속이 가능한 **public 서브넷에 NAT Gateway를 생성**해두고, private 서브넷이 외부 인터넷으로 나아갈 경우에만 사용하도록 라우팅을 추가해두는 것이다.  
public 서브넷과 private 서브넷은 같은 VPC 안에 있으면 서로 통신할 수 있다는 점을 이용하여, NAT 게이트웨이를 생성하면, public 서브넷이 외부 인터넷 데이터를 private 서브넷에게 **대신 전달**해준다.

NAT Gateway는 **내부에서 외부로의 접속만 가능**하며 외부에서 NAT Gateway를 이용하여 접속하는 것은 불가능하다. 이러한 특징으로, 외부 인터넷 연결에 의한 보안 문제도 해결 가능하다.

인터넷 게이트웨이와 비교하면, 인터넷 게이트웨이는 NAT Gateway의 상위 개념이다. 즉 NAT Gateway도 인터넷 게이트웨이 없이는 동작할 수 없다.

참고로 여러 가용영역에 NAT Gateway를 하나만 만들어 공유하게 되면 NAT Gateway 가용영역이 문제가 생기면 다른 쪽도 영향을 받게 된다. 따라서 가용영역 당 NAT Gateway를 만드는 것을 권장하는 편이다.

### NAT Instance
동일한 NAT 역할을 하지만 NAT Gateway와 차이점은,  
NAT Gateway는 AWS에서 제공하는 최신식 서비스이며, NAT Instance는 EC2를 NAT으로 사용하는 구식 기술이다.

NAT Gateway 비용이 NAT Instance에 비해 3배 가까이 차이가 나간다는 점 때문에 알아두는 것 추천

## Bastion Host
앞서 NAT Gateway는 **외부에서 내부로 접속은 못한다**고 했다. 만약 ssh로 외부에서 인스턴스에 접속해 조정할 일이 있다면 어떻게 private 서브넷으로 접속해서 해야할까?  
바로 이러한 역할을 해주는 게 **Bastion Host**이다.

Bastion Host는 private 서브넷과의 통신을 도와주는 대리인이라고 할 수 있다.  
내부와 외부 네트워크 사이에서 일종의 게이트 역할을 수행하는 호스트이다.  
**Bastion Host로 ssh 연결을 한 후 Bastion Host에서 private 서브넷의 호스트에 ssh 연결을 하는 형태**로 private 서브넷에 접근할 수 있게 된다.  
Bastion Host는 게이트웨이같은 서비스를 만들어 등록하는 게 아니라, **private 서브넷에 연결되어 접속할 수 있는 public 서브넷**을 그냥 Bastion Host라고 한다.
