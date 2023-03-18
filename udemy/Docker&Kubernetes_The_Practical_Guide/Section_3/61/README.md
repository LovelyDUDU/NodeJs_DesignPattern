# 61. 환경 변수 & ".env" 파일 작업

도커는 Arguments(인수)와 Environment Variables (환경 변수)를 지원한다.

Argument를 사용하면 Dockerfile 에서 특정 Dockerfile 명령으로 다른 값을 추출하는데 사용할 수 있는 변수를 설정할 수 있다.

Environment 는 arg처럼 Dockerfile 내부에서 사용할 수 있다.

Argument는 실행중인 애플리케이션의 전체 애플리케이션 코드에서 사용할 수 있다.

Dockerfile 내부의 `ENV` 옵션을 설정하여 환경 변수가 존재한다고 Docker에 알린 다음, 

`docker run` 명령에서 `--env` 또는 `-e` 옵션을 사용하여 구체적인 값을 제공할 수 있다.

Dockerfile에서 PORT=80 이라는 환경변수를 설정하는 방법
```bash
...

ENV PORT=80

EXPOSE $PORT

...
```


`--env` 또는 `-e` 옵션을 사용하여 환경 변수를 설정하는 방법
```bash
# 환경 변수를 여러개 설정할때는 -e를 여러번 사용하면 된다.
> docker run -d --rm -p 3000:80 --env PORT=8000 --name feedback-app -v feedback:/app/feedback -v "$(pwd):/app:ro" -v /app/node_modules -v /app/temp feedback-node:volumes
```

`--env` 또는 `-e` 옵션을 사용하는데 `.env` 파일을 사용하는 법
```bash
> docker run -d --rm -p 3000:80 --env-file ./.env --name feedback-app -v feedback:/app/feedback -v "$(pwd):/app:ro" -v /app/node_modules -v /app/temp feedback-node:volumes
```

인수와 환경변수는 서로 다른 모드, 다른 구성에서 하나의 동일한 이미지를 기반으로 하나의 동일한 컨테이너를 실행하는데 도움이 된다.