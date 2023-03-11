# 88. 모듈 요약
## Frontend 컨테이너 실행 명령어
```bash
docker run -v $pwd:/app/src --name goals-frontend --rm -p 3000:3000 -it goals-react
```

## Backend 컨테이너 실행 명령어
```bash
docker run --name goals-backend -v $pwd:/app -v logs:/app/logs -v /app/node_modules -e MONGODB_USERNAME=max -d --rm -p 80:80 --network goals-net goals-node
```

## Mongodb 컨테이너 실행 명령어
```bash
docker run --name mongodb -v data:/data/db --rm -d --network goals-net -e MONGO_INITDB_ROOT_USERNAME=max -e MONGO_INITDB_ROOT_PASSWORD=secret mongo
```

이렇게 명령어들이 너무 긴 문제점이 있어서 이러한 부분에 대해서 추후 다뤄볼 예정이다.