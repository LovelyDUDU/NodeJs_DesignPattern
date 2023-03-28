# 153. 멀티 스테이지 빌드 소개

멀티 스테이지 빌드를 사용하면 파일 내부에 "스테이지(stage)"라고 하는 여러 빌드 단계 또는 설정 단계를 정의하는 하나의 Dockerfile을 가질 수 있다.

스테이지는 서로의 결과를 복사할 수 있으므로 최적화된 파일을 생성하는 스테이지와 그 파일을 제공하는 다른 스테이지를 가진다. 위에서 아래로 단계별로 모든 스테이지를 거쳐 전체 Dockerfile을 빌드할 수도 있다. 또는 멀티 스테이지 빌드와 멀티 스테이지로 부터의 모든 단계를 건너뛰어 빌드하기 위해 개별스테이지를 선태할 수도 있다.

```dockerfile
FROM node:14-apline as build

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# 멀티 스테이지 빌드에서는 CMD 대신 RUN을 사용해야한다.
RUN npm run build 

# build 명령어로 최적화된 파일을 갖게되면 빌드된 파일을 가지고 다른 베이스 이미지로 전환한다.

# nginx는 가볍고 슬림한 웹서버이다. FROM 명령어를 이용하여 멀티 스테이지 빌드용 Dockerfile로 전환할 수 있다. (FROM명령은 새 스테이지를 만든다.)
FROM nginx:stable-alpine

# build라는 이름의 스테이지를 복사한다. --from=build는 build 스테이지에서 최종 컨텐츠를 복사한다는 것이다.
# /app/build 폴더로 부터 데이터를 /usr/share/nginx/html 로 복사한다는 것이다.
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```
