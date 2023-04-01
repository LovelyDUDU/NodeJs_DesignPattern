# 186. 첫 번째 Deployment - 명령적 접근 방식 사용

```bash
# 이미지 빌드
docker build -t kub-first-app .

# cluster 가 실행중인지 확인
minikube status

# cluster에 first-app 이라는 deployment를 생성하라는 지시를 보냄
# 해당 cluster에 있는 마스터 노드(컨트롤 플레인)으로 전송
kubectl create deployment first-app --image=kub-first-app

# cluster에 deployment가 얼마나 있는지 확인
# 0/1 -> 1개의 deployment가 있고 0개가 준비되어 있음
kubectl get deployments
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
first-app   0/1     1            0           11s

# deployment에서 생성된 것들 조회
# 이미지를 만든 것은 로컬이고, 클러스터는 가상머신에서 실행되기 때문에 실패
kubectl get pods
NAME                      READY   STATUS         RESTARTS   AGE
first-app-ccf46d7-8vzxt   0/1     ErrImagePull   0          113s

# deployment 삭제
kubectl delete deployment first-app 

# 만들었던 이미지를 docker hub에 업로드
docker tag kub-first-app harry/kub-first-app
docker push harry/kub-first-app

# 다시 클러스터에 deployment 생성
kubectl create deployment first-app --image=harry/kub-first-app

# 다시 확인
kubectl get pods
NAME                         READY   STATUS              RESTARTS   AGE
first-app-85ffd6dc99-m277m   0/1     ContainerCreating   0          12s

kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
first-app-85ffd6dc99-m277m   1/1     Running   0          18s

# 웹 대시보드 조회
minikube dashboard
```