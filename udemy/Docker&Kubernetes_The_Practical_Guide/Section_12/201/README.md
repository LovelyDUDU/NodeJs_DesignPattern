# 201. Label & Selector에 대한 추가 정보

selector는 리소스에 다른 리소스를 연결하는데 사용한다. 예를들어 deployment에 pod을 연결하거나, service에 경로를 연결한다.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: second-app-deployment
  labels:
    group: example
spec:
  replicas: 1
  selector:
    matchLabels:
      app: second-app
      tier: backend
    # matchExpressions: 
    #   - {key: app, operator: NotIn, values: [second-app, first-app]}
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

matchExpressions는 matchLabels와 달리, label에 키 값 쌍이 여러 개 중첩되지 않으며, 대신 일치하는 객체를 갖기 위해 모두 충족되어야 하는 표현식의 목록을 가집니다.

key: label 키
values: 허용하려는 값
operator: 연산자 (In, NotInm Exists, DoNotExist)
{key: app, operator: NotIn, values: [second-app, first-app]}
-> app label이 [] 안에 있는 값을 갖는 모든 pod을 선택

```bash
# -l: 특정 label만 삭제
# delete <삭제하고자 하는 리소스> -l <삭제할 label>
kubectl delete deployment -l group=example
```