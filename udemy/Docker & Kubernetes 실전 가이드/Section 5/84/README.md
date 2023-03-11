# 84. 효율적인 컨테이너 간 통신을 위한 Docker 네트워크 추가하기

83장에서는 Docker network 없이 애플리케이션을 구동시켰다면, 84장에서는 Docker network 를 사용하여 애플리케이션을 구동한다.

## Docker Network 생성
```bash
# goals-net 이라는 도커 네트워크 형성
docker network create goals-net
```

## Mongo DB 컨테이너에 네트워크 설정
동일한 도커 네트워크 내부에서 통신할 예정이라 포트 번호를 따로 할 필요가 없다.
```bash
docker run --name mongodb --rm -d --network goals-net mongo
```

## Backend 컨테이너에 네트워크 설정
우선 서버 소스코드에서 mongo db와 연결되는 부분을 수정해야 한다.
```javascript
// "mongodb://host.docker.internal:27017/course-goals"
// "mongodb://localhost:27017/course-goals"

// mongo db 컨테이너와 같으 네트워크에서 통신할 예정이라 로컬 호스트 머신을 참조하는 host.docker.internal 대신 mongodb 컨테이너의 이름을 사용하면 된다.
"mongodb://mongodb:27017/course-goals"
```

```bash
# 소스코드 수정후 이미지 재빌드
docker build -t goals-node .
```

```bash
# 재빌드한 이미지로 컨테이너 생성
docker run --name goals-backend --rm -d -p 80:80 --network goals-net goals-node
```

## Frontend 컨테이너에 네트워크 설정
React 코드는 컨테이너 내부에서 실행되지 않고 브라우저에서 실행된다. 그렇기 때문에 React 코드에서 백엔드 api로부터 정보를 가져올 때 `http://goals-backend/goals/`  가 아니라 `http://localhost/goals/` 로부터 가져와야 한다. (브라우저는 `goals-backend` 가 뭔지 모르고 `localhost`만 이해할 수 있다.)

```bash
# 이미지 빌드
docker build -t goals-react .
```

네트워크 옵션 또한 필요없다.
```bash
docker run --name goals-frontend --rm -p 3000:3000 -it goals-react
```