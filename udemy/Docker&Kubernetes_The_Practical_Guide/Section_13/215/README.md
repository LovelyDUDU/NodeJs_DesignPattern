# 215. 볼륨에서 영구(Persistent) 볼륨으로
pod 이 제거되거나 교체되었을때, 혹은 auto scaling 으로 추가되었을때 데이터를 공유하지 못하는 경우는 hostPath로 해결할 수 있다.

근데 hostPath는 단일 노드 환경(e.g. minikube)에서만 사용할 수 있다는 단점이 있다.

pod과 node에 독립적인 볼륨이 필요할 수 있는데 이러한 볼륨이 쿠버네티스에 "persist volume" 이라고 있다.

영구 볼륨(Persist volume)은 Pod 및 노드 독립성에 대한 아이디어를 기반으로 구축됩니다.

영구 볼륨을 사용하는 경우 pod 에 volume 대신 PV Claim(Persist Volume Claim) 이라는 것이 추가되면서 Persist Volume에 도달할 수 있게 된다. 

https://kubernetes.io/docs/concepts/storage/persistent-volumes/

PV 유형에는 emptyDir이 없으며, hostPath 는 minikube와 같이 단일 노드 환경에 대해서만 사용할 수 있게 제한된다

Cluster
- Node
  - Pod
  - Pod
  - PV Claim (PV에 액세스 요청)

- Persist Volume