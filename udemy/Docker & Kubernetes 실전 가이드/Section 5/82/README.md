# 82. Node 앱 도커화 하기

```Dockerfile
FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD ["node", "app.js"]
```

`/backend/app.js` 에서 mongodb와 연결하는 부분이 `localhost` 이면 에러가 발생한다. 왜냐하면 `mongoDB`는 컨테이너화되어 포트 27017에 노출되어있는데 백엔드는 로컬호스트에 연결하려고 했기 때문에 에러가 발생했다.

```
FAILED TO CONNECT TO MONGODB
MongooseServerSelectionError: connect ECONNREFUSED 127.0.0.1:27017
    at Connection.openUri (/app/node_modules/mongoose/lib/connection.js:847:32)
    at /app/node_modules/mongoose/lib/index.js:351:10
    at promiseOrCallback (/app/node_modules/mongoose/lib/helpers/promiseOrCallback.js:10:12)
    at Mongoose._promiseOrCallback (/app/node_modules/mongoose/lib/index.js:1149:10)
    at Mongoose.connect (/app/node_modules/mongoose/lib/index.js:350:20)
    at Object.<anonymous> (/app/app.js:81:10)
    at Module._compile (node:internal/modules/cjs/loader:1275:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1329:10)
    at Module.load (node:internal/modules/cjs/loader:1133:32)
    at Module._load (node:internal/modules/cjs/loader:972:12)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:83:12)
    at node:internal/main/run_main_module:23:47 {
  reason: TopologyDescription {
    type: 'Single',
    setName: null,
    maxSetVersion: null,
    maxElectionId: null,
    servers: Map(1) { 'localhost:27017' => [ServerDescription] },
    stale: false,
    compatible: true,
    compatibilityError: null,
    logicalSessionTimeoutMinutes: null,
    heartbeatFrequencyMS: 10000,
    localThresholdMS: 15,
    commonWireVersion: null
  }
}
```
그렇기 때문에 백엔드 코드에서 `localhost` 부분을 `host.docker.internal` 로 수정해야한다. (`host.docker.internal`은 도커에 의해 실제 로컬호스트 머신 IP로 변환되는 특수 주소이다.)

```bash
# 백엔드 컨테이너를 위한 이미지 생성 (소스코드가 수정되었을 경우 재빌드)
docker build -t goals-node .

# 컨테이너 실행
# -p 옵션으로 로컬 80번포트와 컨테이너 80번 포트를 매핑
docker run --name goals-backend --rm -d -p 80:80 goals-node
```