# 86. NodeJS 컨테이너의 볼륨, 바인딩 마운트 및 폴리싱(Polishing)

NodeJS 컨테이너(백엔드 컨테이너)가 작성하는 로그 파일에 데이터가 유지되도록하고, 실시간으로 소스 코드 업데이트가 이루어지게 할 예정.


## 로그 파일에 데이터가 유지되게

더 긴 컨테이너 내부 경로가 우선하고, 더 짧은 내부 경로를 덮어쓴다.

`-v $(pwd):/app`: 바인드 마운트 사용. 로컬 호스팅 디렉토리를 컨테이너 내부의 `/app` 에 바인딩

`-v logs:/app/logs`: named volume 사용. `/logs` 폴더와 컨테이너 내부의 `/app/logs` 를 바인딩

`-v /app/node_modules`: 익명 volume 사용
```bash
# 이미지 
docker build -t goals-node .

# logs 라는 named volume을 만들고 `/app/logs` 에 바인딩
docker run --name goals-backend -v $pwd:/app -v logs:/app/logs -v /app/node_modules -e MONGODB_USERNAME=max -d --rm -p 80:80 --network goals-net goals-node
```

## 실시간으로 소스 코드 업데이트가 이루어지게 
`package.json` 에서 `nodemon`을 설치하고 scripts를 수정하면 된다.

```
# package.json
...
"scripts": {
  ...
  "start": "nodemon app.js",
  ...
},
...
"devDependencies": {
  "nodemon": "^2.0.4",
}
dev
...
```

```Dockerfile
# CMD ["node", "app.js"]

CMD ["npm", "start"]
```

## DB 정보 분리
```Dockerfile
# Dockerfile에 env 추가
...
ENV MONGODB_USERNAME=root
ENV MONGODB_PASSWORD=secret
```

```javascript
// "mongodb://max:secret@mongodb:27017/course-goals?authSource=admin",

`mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@mongodb:27017/course-goals?authSource=admin`,
```

## .dockerignore 추가
```
node_modules
Dockerfile
.git
```

<hr>

### AuthenicationFailed
85장에서 86장 실습을 바로 했을때 AuthenicationFailed 에러를 만났다. 이때 `docker volume rm data` 를 하고 다시 진행하면 잘 된다. (`data` 는 Mongo DB 의 컨테이너 볼륨 이름이다.)