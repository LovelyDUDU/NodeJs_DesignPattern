# 196. Pod와 컨테이너 사양(Specs) 추가

```yaml
apiVersion: apps/v1
kind: Deployment # 생성하고자 하는 k8s 객체의 종류 - Deployment, Service, Job 등
metadata: 
  name: second-app-deployment # 객체 이름
spec: # deployment의 사양 -> 원하는 pod과 pod 수를 설정
  replicas: 1 # pod 인스턴스의 수
  selector:
    matchLabels:
      app: second-app
      tier: backend
  template: # 생성되어야 하는 pod을 정의
    # kind: Pod # deployment template은 항상 pod에 대해 설명하기 때문에 kind를 굳이 쓸 필요가 없다. (PodTemplateSpec) 
    # FYI, https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#deployment-v1-apps
    metadata: 
      labels:
        app: second-app # pod 이름
        tier: backend
    spec: # pod의 사양
      containers:
        - name: second-node # 컨테이너 이름
          image: harry/kub-first-app:2 # --image로 지정했던 이미지
        # - name: ... -> "-" 로 여러개의 컨테이너를 정의할 수 있다.
        #   image: ...

```

```bash
# apply: 연결된 클러스터에 구성 파일을 적용
# -f: 파일을 식별
kubectl apply -f=deployment.yaml
```
