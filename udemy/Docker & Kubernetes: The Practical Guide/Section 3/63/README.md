# 63. 빌드 인수(ARG) 사용하기

```dockerfile
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# ARG = 빌드 타임 인수
ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT

# $ -> PORT 가 환경변수를 참조하고 있음을 알림
EXPOSE $PORT

CMD ["npm", "start"]
```

Dockerfile에 ARG(빌드 타임 인수)를 선언하여 사용하면 이미지를 빌드할 때 변경되지 않은 동일한 하나의 Dockerfile을 기반으로 하여 다른 디폴트 값으로 하드코딩없이 여러 번 이미지를 빌드할 수 있다. (예를 들면 포트)

`ARG DEFAULT_PORT=80` 는 Dockerfile에서만 사용이 가능하고, 소스코드에서는 호출이 불가능하다. ARG는 컨테이너가 시작될 때 실행되는 런타임 명령이다.

`DEFAULT_PORT`를 Dockerfile 수정없이 다른 포트로 빌드할때는 
`docker build -t feedback-node:dev --build-arg DEFAULT_PORT=8000` 으로 가능하다.