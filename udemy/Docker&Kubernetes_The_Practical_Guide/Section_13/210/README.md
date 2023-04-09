# 210. 새 Deployment & Service만들기
```bash
# deployment가 있는지 확인
kubectl get deployments

# minikube 확인
minikube status

# minikube 시작 (종료는 minikube stop)
minikube start --driver

# service, deployment 실행
kubectl apply -f=service.yaml -f=deployment.yaml

# 서비스 시작
minikube service story-service
```


```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: story-deployment
spec: 
  replicas: 1
  selector:
    matchLabels:
      app: story # "template.metadata.labels.app: story" 을 찾기위함
  template: # deployment에 의해 생성되는 pod에 대한 pod template을 만들어야 함.
    metadata:
      labels: # pod을 선택할 수 있는 label 설정
        app: story
    spec:
      containers: # 어떤 컨테이너가 이 pod의 일부가 될 지 
        - name: story # 컨테이너 이름
          image: academind/kub-data-demo
```


```yaml
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: story-service
spec:
  selector: 
    app: story
  type: LoadBalancer
  ports:
    - protocol: "TCP"
      port: 80
      targetPort: 3000
```