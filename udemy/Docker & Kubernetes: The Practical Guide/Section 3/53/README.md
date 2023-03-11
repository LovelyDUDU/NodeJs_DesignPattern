# 53. 다른 볼륨 결합 & 병합하기
Dockerfile이 다음과 같을 때
```
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD ["node", "server.js"]
```

```
docker build -t feedback-node .

docker run -d -p 3000:80 --rm --name feedback-node -v feedback:/app/feedback -v "$(pwd):/app" feedback-node:latest
```
이렇게 바인드 마운트를 하면 에러가 발생한다.

```
internal/modules/cjs/loader.js:934
  throw err;
  ^

Error: Cannot find module 'express'
```

왜냐하면 `${pwd}:/app` 을 하면 `${pwd}`(로컬 폴더) 에 있는 모든 파일이 `/app`에 바인딩 되는데, 로컬 폴더에는 `/node_modules` 파일이 없기 때문이다. (Dockerfile에서는 package.json으로 부터 dependency를 생성하는 과정이 있지만 현재 바인드 마운트에는 없다.)

컨테이너 내부의 `app` 폴더에 `/node_modules` 파일을 가지고 있는 상태이다. (`RUN npm install` 명령 때문에)

그리고 로컬 머신(컨테이너 외부) 에는 `/node_modules` 파일이 없다.

컨테이너 내부의 파일이 로컬 파일을 덮어쓰지는 않지만 로컬 파일이 컨테이너 내부의 파일을 덮어쓰게 된다. 즉 `/node_module` 가 없어진다.

이렇게 `/node_modules` 폴더가 없어지는 문제를 해결하기 위해서는 도커에게 내부 파일 시스템에 특정 부분이 있다는 것을 알려야 한다. (로컬 머신으로 부터 덮어쓰면 안되는 부분이 있다는 것을 알려야 한다.)

이때 익명볼륨을 사용한다. `-v /app/node_modules` 로 `/node_modules` 폴더를 바인딩한다. 경로 앞에 콜론(:)를 추가하여 볼륨 이름을 지정할 수도 있다.
(콜론 앞에 로컬 머신경로가 붙으면 바인드 마운트, 콜론 앞에 경로가 아닌 것이 붙으면 볼륨이름으로 취급되어 named volume 으로 구분된다.)

`-v /app/node_modules` 는 Dockerfile에서의 `VOLUME [ "/app/node_modules ]`와 동일하다.

```
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "$(pwd):/app" -v /app/node_modules feedback-node:latest
```

도커는 매핑하려는 경로가 여러개인 경우 더 구체적인 경로를 채택한다.

`-v ${pwd}:/app` 과 `-v /app/node_modules` 를 두고 봤을때 `/app` 폴더에 바인딩 하지만 `/node_modules` 또한 제거하지 않고 바인딩한다. (바인드 마운트된 볼륨의 내용물에 `-v /app/node_modules` 를 덮어쓴다.)



