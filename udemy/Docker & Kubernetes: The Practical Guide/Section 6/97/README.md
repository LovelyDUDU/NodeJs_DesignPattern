# 97. 또다른 컨테이너 추가하기

```yaml
# version: 도커 컴포즈 사양의 버전 
# FYI, https://docs.docker.com/compose/compose-file/compose-file-v3/

# services: 여러 하위 요소(container) 를 가질 수 있다.

version: "3.8"
services:
  mongodb: # 서비스 이름은 코드에서 사용할 수 있는 이름이다.
    image: "mongo"
    volumes:
      - data:/data/db
    # environment: # case 1 > not use .env, use ": " or "="
    #   MONGO_INITDB_ROOT_USERNAME: max # case 1-1
    #   MONGO_INITDB_ROOT_PASSWORD: secret
      # - MONGO_INITDB_ROOT_USERNAME=max # case 1-2
      # - MONGO_INITDB_ROOT_PASSWORD=secret
    env_file: # case 2 > use .env
      - ./env/mongo.env
  backend:
    build: ./backend # 빌드해야 하는 Dockerfile의 위치
    # build: 
    #   context: ./backend 
    #   dockerfile: Dockerfile # Dockerfile의 이름이 Dockerfile이 아닌경우 사용
    ports:
      - "80:80" # host: 80, container inner port: 80
    volumes:
      - logs:/app/logs # named volume
      - ./backend:/app # bind mount. backend 폴더에 있는 모든 것을 컨테이너 내부의 app 폴더와 공유
      - /app/node_modules # annonymus volume
    env_file:
      - ./env/backend.env
    depends_on: # 하나의 컨테이너가 다른 컨테이너에 의존할 때
      - mongodb
  frontend:
    build: ./frontend
    ports: 
      - "3000:3000"
    volumes:
      - ./frontend/src:/app/src
    stdin_open: true
    tty: true # terminal 에 연결하기 위함
    depends_on:
      - backend


volumes: # named volume 만 추가
  data:
  logs:
```