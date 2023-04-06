# 198. 선언적으로 Service 만들기

https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#service-v1-core

```yaml
apiVersion: v1 # core/v1
kind: Service
metadata:
  name: backend
spec:
  selector: # 이 service 가 app: second-app label에 있는 모든 pod을 선택하고 노출한다. 
  # = 이 deployment에 의해 생성된 pod가 이 service에 의해 노출된다.
    app: second-app
  ports:
    - protocol: 'TCP'
      port: 80 # 외부 포트
      targetPort: 8080
    # - protocol: 'TCP'
    #   port: 443
    #   targetPort: 443
  type: LoadBalancer
```

selector는 이 리소스에게 제어되거나, 연결되어야 하는 다른 리소스를 식별하는 역할을 한다.

service 객체는 pod를 클러스터나 외부에 노출한다. 따라서 service로 deployment를 제어하지 않고 pod으로 제어한다.


```bash
kubectl apply -f service.yaml

minikube service backend
```