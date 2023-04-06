# 192. Deployment 업데이트 하기

이미지를 업데이트 하려면 `kubectl set image` 명령어를 사용하면 된다.
```bash
# 이미지 재빌드
docker build -t harry/kub-first-app .

# docker hub에 업로드
docker push harry/kub-first-app

# deployment를 업데이트 하기전에 deployment가 있는지 확인
kubectl get deployments

# 현재 어떤 컨테이너와 이미지를 어떤 미래의 이미지로 업데이트해야 하는지 알림
kubectl set image deployment/first-app kub-first-app=harry/kub-first-app

# deployments를 확인해도 별다른 변화가 없는데 이유는 새 이미지에 다른 태그가 없기 때문이다
kubectl get deployments

# 다시 dockerhub에
docker build -t harry/kub-first-app:2 .
docker push harry/kub-first-app:2

# 이미지가 업데이트 되었다고 알려줌
kubectl set image deployment/first-app kub-first-app=harry/kub-first-app:2
deployment.apps/first-app image updated

# 성공적으로 rollout 됨
kubectl rollout status deployment/first-app
deployment "first-app" successfully rolled out
```