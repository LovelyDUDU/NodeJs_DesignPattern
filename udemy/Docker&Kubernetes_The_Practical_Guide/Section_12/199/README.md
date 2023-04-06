# 199. 리소스 업데이트 & 삭제

만약 yaml 파일에 수정이 필요하다면 수정을 하고 다시 명령어를 적용하면 된다.

```bash
kubectl apply -f=deployment.yaml
```

리소스 삭제 방법
```bash
kubectl delete deployment <name>

# 혹은

# deployment.yaml 으로 생성된 리소스 삭제
kubectl delete -f=deployment.yaml
```