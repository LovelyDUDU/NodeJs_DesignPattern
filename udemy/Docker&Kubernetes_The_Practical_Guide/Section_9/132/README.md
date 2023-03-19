# 132. 로컬 이미지를 클라우드로 푸시(push)하기

```bash 
docker build -t node-dep-example-1 --platform linux/amd64 .

docker tag node-dep-example-1 harry/node-example-1

docker push harry/node-example-1
```
