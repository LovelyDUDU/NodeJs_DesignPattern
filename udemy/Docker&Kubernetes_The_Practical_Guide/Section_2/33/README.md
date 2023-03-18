# 33. 중지된 컨테이너 자동 제거하기

`--rm` 은 컨테이너가 종료될 때 자동으로 제거하게 해주는 옵션이다. 컨테이너를 시작할 때 추가해주면 된다.

```
user@MacBookPro DOCKER-COMPLETE % docker run -p 3000:80 -d --rm d164d5ea2c7c 
53a17a0f82b25f29e3535ae7b9c3f599bb8d502db5048cf956c6b12765217dcd
user@MacBookPro DOCKER-COMPLETE % docker stop
user@MacBookPro DOCKER-COMPLETE % 
user@MacBookPro DOCKER-COMPLETE % docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                  NAMES
53a17a0f82b2   d164d5ea2c7c   "docker-entrypoint.s…"   41 seconds ago   Up 41 seconds   0.0.0.0:3000->80/tcp   beautiful_faraday
user@MacBookPro DOCKER-COMPLETE % docker stop 53a17a0f82b2
53a17a0f82b2
user@MacBookPro DOCKER-COMPLETE % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## 정리

```bash 
# --rm 옵션을 넣으면 컨테이너가 중지됐을때 바로 삭제된다.
docker run -p 3000:80 -d --rm d164d5ea2c7c 
```