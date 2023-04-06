# 195. 배포 구성 파일 생성하기 (선언적 접근방식)

yaml 파일에서 deployment이 어떠한 지, 어떻게 작동되어야 하는지, 어떻게 구성되어야 하는지를 지정하면 된다.

https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/


```yaml
apiVersion: apps/v1
kind: Deployment # 생성하고자 하는 k8s 객체의 종류 - Deployment, Service, Job 등
metadata: 
  name: second-app-deployment # 객체 이름
spec: # 사양
  replicas: 1
  selector:
    matchLabels:
      app: second-app
      tier: backend
  template:
    metadata: 
      labels:
        app: second-app
        tier: backend
    spec: 
      containers:
        - name: second-node
          image: academind/kub-first-app:2
        # - name: ...
        #   image: ...

```