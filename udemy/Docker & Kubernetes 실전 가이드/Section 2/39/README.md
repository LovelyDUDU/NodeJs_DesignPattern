# 39. 공유 이미지 가져오기(pull) & 사용하기

DockerHub로 부터 공유 이미지를 가져올때는
```bash
# docker pull academined/node-hello-world
docker pull <repository/image name>
```

`docker pull`은 항상 컨테이너 registry에서 그 이름의 최신 이미지를 가져온다.

`docker pull` 말고 `docker run` 을 사용하면 로컬에 해당 이미지가 있는지 확인해보고 없으면 이미지 이름을 사용한 container history에 접근해서 가져온다. 단, `docker run`으로 이미지를 가져올 경우, 최신 업데이트 이미지를 제공받지는 못한다.