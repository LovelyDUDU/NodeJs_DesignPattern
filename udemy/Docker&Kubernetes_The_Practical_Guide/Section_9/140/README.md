# 140. 관리되는 컨테이너를 업데이트하기

ECS 에서 실행중인 컨테이너를 업데이트하기 위해서 우선 이미지 파일을 재빌드하고 docker hub에 push 한다.

```bash
# 재빌드
docker build -t node-dep-example-1 --platform linux/amd64 .

docker tag node-dep-example-1 harrykim/node-example-1

docker push harrykim/node-example-1
```

그리고 Clusters 로 이동해서 해당 클러스터의 "Cluster Definition" 으로 이동한 후, "Create new revision (새 계정 생성)" 에 들어간다. 옵션 그대로 생성하면 새로운 태스크가 생기고 이전 태스크는 사라진다. (public ip도 변경된다.)