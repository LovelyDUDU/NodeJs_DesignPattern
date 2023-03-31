# 173. Kubernetes: 아키텍처 & 핵심 개념

### Pod (Container)
- k8s의 가장 작은 단위. config file에서 정의할 수 있다.
- 항상 함께 작동해야 하는 여러 컨테이너를 보유할 수 있다.
- pod은 k8s에 의해 사용 가능한 모든 Worker node로 자동으로 배포된다.

### Worker Node
- 컨테이너를 실행하는 k8s 내의 것
- 머신, 가상 인스턴스 같은 것
- 특정 양의 CPU와 메모리가 있는 어딘가의 머신이며 그 머신에서 pod을 실행할 수 있다.
- 동일한 worker node 중 하나에서 둘 이상의 pod을 실행할 수 있다.
- k8s로 작업할 때 일반적으로 하나 이상의 워커 노드가 필요하다.

### Proxy
- worker node의 pod 네트워크 트래픽의 제어를 설정하는 도구
- pod이 인터넷에 연결할 수 있는지의 여부와 pod 내부에서 실행되는 컨테이너를 외부에서 어떻게 접근할 수 있는지를 제어한다.
- 예를 들어, pod의 컨테이너에서 웹 애플리케이션을 실행하는 경우 사용자의 외부 트래픽이 이 컨테이너에 도달할 수 있도록 프록시를 구성해야 한다.

### Master Node
- worker node와 상호작용하여 제어하는 컨트롤 센터
- master node는 다른 서버, 다른 리모트 머신이다.
- 실행중인 Control plane을 가지고 있으며, worker node와 worker node 상에서 실행중인 pod와 상호작용하는 책임을 가진다.
- 하나의 worker node가 다운되어도 master node가 함께 다운되지는 않는다.