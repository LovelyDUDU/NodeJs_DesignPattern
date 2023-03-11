# 38. DockerHub에 이미지 푸시(push)하기

이미지를 공유(push)하는 곳은 크게 두가지 있다.
1. Docker Hub
   - 공식 도커 이미지 레포지토리
   - Public, Private 그리고 공식적인 Images 가 있다.

2. Private Registry
   - provider 및 registry가 많다.
   - 사용자 (또는 팀) 내에서만 이미지를 사용할 수 있다.

이미지를 공유할 때
```bash
docker push <image name>
```

이미지를 사용할 때
```bash
docker pull <image name>
```

이미지를 push/pull할때는 해당 provider의 URL(HOST:NAME) 이 포함되어 있어야 한다.

이미지 이름(태그) 변경할 때 
```bash
# 이름뒤에 태그는 default값이 latest 이다.
# docker tag goals:latest seunghwankim/node-hello-world
docker tag <이전 이름> <새 이름>
```

대신 이름을 변경하더라도 이전에 있던 이미지는 그대로 존재한다 (복제)