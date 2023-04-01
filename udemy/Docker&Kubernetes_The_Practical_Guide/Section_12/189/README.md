# 189. Service로 Deployment 노출하기

`kubectl creat service` 명령어로 service를 생성할 수 있긴한데 `kubectl expose`라는 더 편한 명령어가 있다.

```bash
# first-app: deployment 이름
# --port: 노출하려는 포트
# --type: 만들고자 하는 service 또는 expose 유형 
# ClusterIP: (디폴트) 클러스터 내부에서만 연결할 수 있는 유형
# NodePort: deployment가 실행중인 워커 노드의 IP주소를 통해 노출되는 유형
# LoadBalancer: 인프라에 존재하는 LoadBalancer를 활용하는데 service에 대한 고유한 주소를 생성하고, 들어오는 트래픽을 이 service의 일부인 모든 pod에 고르게 분산하게 함 (클러스터와 클러스터가 실행되는 인프라가 지원하는 경우에만 사용 가능. e.g. AWS, minikube)
kubectl expose deployment first-app --type=LoadBalancer --port=8080 

# 서비스 확인
# kubernetes는 자동으로 생성된 디폴트 service
# first-app 의 외부 IP가 pending인데 이유는 minikube가 로컬에서 실행되는 가상머신이라 그렇다.
kubectl get services 
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
first-app    LoadBalancer   10.109.141.161   <pending>     8080:31935/TCP   9s
kubernetes   ClusterIP      10.96.0.1        <none>        443/TCP          12h

# 애플리케이션을 보는데 사용할 수 있는 주소가 포함된 정보를 얻음 (minikube 전용)
minikube service first-app

# first-app은 k8s 클러스터로 보낸 deployment를 기반으로 k8s에 의해 생성된 pod에서 실행된다.
```