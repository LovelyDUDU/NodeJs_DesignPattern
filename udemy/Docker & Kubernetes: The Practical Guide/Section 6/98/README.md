# 98. 이미지 빌드 & 컨테이너 이름 이해하기

`docker-compose up --build`: 이미지를 강제로 리빌드 할 수 있다.

`docker-compose build`: 누락된 이미지를 빌드만 하고 컨테이너를 실행하지는 않는다.

서비스 하위에 `container_name` 값을 추가하면 컨테이너 이름이 강제된다.

```yaml
...
servicies: 
  mongodb:
    ...
    container_name: mongo
    ...
```
