# 106. Docker Compose 사용

`docker-compose up` 은 docker-compose.yaml 파일에 정의된 서비스를 불러오기 위함이다. 

`docker-compose run` 은 docker-compose.yaml 파일에 정의된 서비스를 단일 서비스로 실행할 수 있다.

즉, docker-compose 에 작성된 npm 이라는 이름의 service 를 실행시키고자 할 때는 `docker-compose run npm <명령어>` 으로 해야 한다.

```bash
docker-compose run npm init
```

`docker-compose down` 을 하면 자동으로 컨테이너가 제거된다.