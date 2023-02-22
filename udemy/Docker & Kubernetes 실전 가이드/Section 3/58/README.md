# 58. "COPY" 사용 vs 바인드 마운트 사용

Dockerfile에서

```
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD ["npm", "start"]
```

`COPY . .` 을 주석처리해도 `docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "$(pwd):/app:ro" -v /app/node_modules -v /app/temp feedback-node:volumes` 이 명령어에 있는 바인드 마운트가 있으면 `localhost:3000` 이 여전히 정상작동한다.

왜냐하면 바인드 마운트로 전체 폴더를 볼륨으로 사용하기때문에 이미지가 생성될 때 `COPY . .` 로 스냅샷을 복사하지 않아도 정상작동한다.

근데 `COPY . .` 대신 바인드 마운트를 쓰면 안된다.


왜냐하면 `COPY . .` 를 안쓰게되면 스냅샷 이미지를 만들 수 없기 때문이다. 그렇기에 prod 용 snapshot container를 생성하는 옵션인 필수다.

+ `docker run` 명령은 개발 중에 사용하는 명령어로 코드의 변경 사항을 실행중인 컨테이너에 즉시 반영한다. 