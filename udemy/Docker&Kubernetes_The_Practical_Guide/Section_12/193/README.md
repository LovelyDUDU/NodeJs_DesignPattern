# 193. Deployment 롤백 & 히스토리
k8s는 새 pod이 구동되어 실행되기 전에 이전 Pod을 종료하지 않는다.

새로 띄우고자 하는 pod에 오류가 생겨 롤백해야 하는 경우가 있을 수 있다.

```bash
# 최근의 deployment가 undo 된다.
kubectl rollout undo deployment/first-app 

# deployment 히스토리 조회
kubectl rollout history deployment/first-app 

# 특정 버전의 deployment 조회
kubectl rollout history deployment/first-app --revision=3

# 특정 버전의 deployment로 롤백
kubectl rollout undo deployment/first-app --to-revision=1

# 서비스 삭제
kubectl delete service first-app
```