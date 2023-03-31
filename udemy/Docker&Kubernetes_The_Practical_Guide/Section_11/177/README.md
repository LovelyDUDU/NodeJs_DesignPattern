# 177. 중요 용어 & 개념

Cluster: master node, worker node(컨테이너형 어플리케이션) 를 구성하는 노드 시스템 세트

Nodes: 하나 또는 여러개의 pod을 호스팅하는 특정 하드웨어 용량을 가지며, cluster와 통신하거나, cluster 내에서 통신하는 물리적인 머신 혹은 가상머신

Master node: 모든 worker node를 걸쳐 pod을 관리하는 control plane를 가지는 노드

Worker node: pod을 호스팅하는 실제 머신. 앱 컨테이너와 이러한 컨테이너에 필요한 리소스를 실행한다. 

Pods: 실제로 실행 App Containers를 보유하고 있으며, 그들이 필요한 자원(required resources, e.g. volumes)가 있다. pod이 생성되는 것은 pod에서 컨테이너를 실행하는 것과 같다.

Containers: 일반 Docker 컨테이너

Services: 고유한 Pod 및 container에 독립적인 IP 주소를 가진 pod 그룹. k8s에서 특정 pod을 외부에 노출하여 특정 IP 주소 또는 도메인으로 특정 pod에 연결할 수 있도록 한다.