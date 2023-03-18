# 104. 첫 번째 유틸리티 컨테이너 구축

```bash
docker build -t node-util .

docker run -it -v $pwd:/app node-util npm init

docker run -it -v "/$pwd:/app" node-util npm init

# 이렇게 해야 되더라 ㅡㅡ
# source -> host, target -> the path of container to mount to the host file path 
docker run \
-it \
--mount type=bind,source="/Users/harry/Project/TIL/udemy/Docker&Kubernetes_The_Practical_Guide/Section_7/104/docker-complete",target=/app \
node-util npm init

# 아니면 이렇게... path 에 띄워쓰기가 있으면 제대로 bind mount가 안되는 것 같아서 디렉토리에 띄워쓰기 모두 삭제함
docker run -it -v "/Users/harry/Project/TIL/udemy/Docker&Kubernetes_The_Practical_Guide/Section_7/104/docker-complete":/app node-util npm init
```
