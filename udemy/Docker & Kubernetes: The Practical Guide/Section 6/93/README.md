# 93. Compose 파일 구성(configuration) 자세히 알아보기

```yaml
# version: 도커 컴포즈 사양의 버전 
# FYI, https://docs.docker.com/compose/compose-file/compose-file-v3/

# services: 여러 하위 요소(container) 를 가질 수 있다.

version: "3.8"
services:
  mongodb:
    image: 'mongo'
    volumes:
      - data:/data/db
    # environment: # case 1 > not use .env, use ": " or "="
    #   MONGO_INITDB_ROOT_USERNAME: max # case 1-1
    #   MONGO_INITDB_ROOT_PASSWORD: secret
      # - MONGO_INITDB_ROOT_USERNAME=max # case 1-2
      # - MONGO_INITDB_ROOT_PASSWORD=secret
    env_file: # case 2 > use .env
      - ./env/mongo.env
    networks: # docker compose를 사용하면 도커가 이 compose 파일에 특정된 모든 서비스에 대해 새 환경을 자동으로 생성하고 모든 서비스를 즉시 그 네트워크에 추가한다. 즉 하나의 동일한 compose 파일에 정의된 서비스들은 동일한 네트워크의 일부가 되기 때문에 굳이 네트워크를 지정할 필요는 없다.
      - goals-net
  backend:

  frontend:
volumes: # volumes는 services 와 동일한 indent에 있어야 한다. servicies에서 사용중인 named-volume 들을 나열하면 된다. 익명 볼륨과 바인드 마운트는 여기에 지정할 필요 없다.
  data: # 다른 서비스에서 동일한 볼륨 이름을 사용하면 그 볼륨이 공유된다. 즉 다른 컨테이너가 호스팅 머신 상의 동일한 볼륨, 동일한 폴더를 사용할 수 있다는 것이다.
```