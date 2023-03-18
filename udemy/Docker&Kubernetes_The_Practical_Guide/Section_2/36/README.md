# 36. 컨테이너와 이미지에 이름 지정 & 태그 지정하기

컨테이너에 이름을 달아줄 때는 `--name` 옵션을 사용하면 된다.

```bash
# --name 뒤에 컨테이너에 붙여줄 이름을 추가하고 그 뒤에 이미지id를 넣어주면 된다.
docker run -p 3000:80 -d --rm --name goalsapp d164d5ea2c7c
```

이미지에도 이름을 달아줄 수 있는데 이를 이미지에서는 `TAG` 라고 부른다.

실제로 이미지 태그는 `name:tag` 처럼 두 부분으로 구성되면 콜론(:)으로 구분된다. 

name은 이미지의 일반적인 이름을 설정할 때 사용한다. name으로 여러개의 특정화된 이미지 그룹을 만들 수 있다.

tag는 이미지의 보다 특정화된 버전을 정의할 수 있다.

예를 들어 name는 "node", tag는 "14" 이 된다.

```bash
# docker build -t <name>:<tag> .
user@MacBookPro DOCKER-COMPLETE % docker build -t goals:latest .

user@MacBookPro DOCKER-COMPLETE % docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
goals        latest    3e78eb7730ad   4 seconds ago   955MB
```

이미지에 이름와 태그가 생기면, 해당 이미지를 기반으로 한 컨테이너를 실행시킬때도 

```
docker run -p 3000:80 -d --rm --name goalsapp goals:latest
```
와 같이 이미지의 이름으로 컨테이너를 실행시킬 수 있다.

태그된 이미지를 포함하여 모든 이미지를 삭제할때는 `docker image prune -a` 명령어를 사용해야 한다.

## 정리
컨테이너에 이름 붙여줄 때 
```bash
# docker run --name <컨테이너 이름> <이미지 id>
docker run -p 3000:80 -d --rm --name goalsapp d164d5ea2c7c
```

이미지에 이름 붙여줄 때
```bash
# docker build -t <repository>:<태그> .
docker build -t node:latest .

# docker build -t node:14 .  처럼 버전별로 관리할 때 용이하다.
```