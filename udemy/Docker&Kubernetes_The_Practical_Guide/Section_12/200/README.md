# 200. 다중 vs 단일 Config 파일

여러 yaml 파일 (리소스 파일)을 하나의 파일로 관리하면서 구분할때는 --- 로 구분할 수 있다.

리소스는 위에서 아래로 생성되기 때문에 yaml 파일의 상단에 service를 배치하는 것이 좋다. (service에 selector가 있기 때문에 그 이후에 생성되는 모든 부분은 동적으로 추가된다.)

```bash
kubectl delete -f=deployment.yaml -f=service.yaml

kubectl apply -f=master-deployment.yaml

minikube service backend
```