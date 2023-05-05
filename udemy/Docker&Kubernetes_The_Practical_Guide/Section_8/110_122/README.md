# 110 ~ 122

Laravel 은 PHP 프레임워크인데 Laravel 개발을 위해 로컬 머신에 환경을 세팅하는 것들이 번거롭다.

이번 모듈의 목표는 로컬 머신에 도커로만 애플리케이션을 빌드하는 것이다. 별다른 설치없이 도커로만 애플리케이션을 빌드하려면 이전 모듈에서 배운 "유틸리티 컨테이너"를 사용해야 한다.

## 필요한 컨테이너 및 역할
- Nginx Web Server: Ningx가 들어오는 요청을 받으면 이 요청을 PHP Interpreter 로 이동시키고 PHP Interpreter에서 응답을 생성하여 클라이언트에게 전달한다. 
- PHP Interpreter: 내부에 PHP가 설치되어 있어서 이 컨테이너를 이용하여 로컬의 소스코드에 접근하게 한다.
- MySQL Database: PHP Interpreter가 MySQL과 통신할 것이다.
- (Utility) Composer: PHP와 관련된 것인데 Node에서 npm 과 같은 것이다. (써드파트 패키지를 설치하기 위한 패키지 매니저)
- (Utility) Laravel Artisan: DB에 대해 migration하고, 초기 시작데이터를 db에 쓰는데 사용하는 명령
- (Utility) npm: front-end 로직의 일부에 npm을 사용할 예정.

## docker-compose.yaml
```yaml
version: '3.8'

services:
  server: # nginx
    # image: 'nginx:stable-alpine'
    build:
      context: . # dockerfile이 dockerfile 폴더가 아닌 메인 프로젝트 폴더에 빌드되기 때문에 context와 더 많은 폴더 사용 가능
      dockerfile: dockerfiles/nginx.dockerfile
    ports:
      - '8000:80' # local:8000, container:80
    volumes:
      - ./src:/var/www/html # php 파일이 서버에 노출되어야 함. conf -> root /var/www/html/public;
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro # 컨테이너가 nginx.conf 파일을 변경하면 안되기 때문에 읽기전용으로 설정한다. 
      # FYI, https://hub.docker.com/_/nginx
    depends_on:
      - php
      - mysql
  php: # PHP Interpreter
    # image: 'php:8.0-fpm-alpine'
    build:
      context: . # dockerfile을 찾을 수 있는 폴더 설정
      dockerfile: dockerfiles/php.dockerfile # 커스텀 Dockerfile 사용 -> 필요한 것을 갖춘 공식 이미지가 없기 때문
    volumes:
      - ./src:/var/www/html:delegated # 소스코드는 컨테이너 내부 폴더에서 사용할 수 있어야 하기 때문에 bind mount
      # delegated: 컨테이너가 일부 데이터를 기록해야하는 경우에 결과를 host 에 즉시 반영하지 않고 batch로 기본처리함으로써 성능이 향상됨 (최적화하는 대신 안정성이 떨어짐)
    # 포트는 nginx.conf 에 9000으로 정의되어 있다. php 컨테이너와 nginx 컨테이너가 통신하기 때문에 별도의 포트 설정이 없다.
    # 호스트 머신에서 php 컨테이너와 상호작용하려는 경우에는 3000:9000 으로 매핑하면되는데 지금은 다른 컨테이너 간의 통신이라 매핑이 필요없다.
  mysql: # DB
    image: mysql:5.7
    env_file:
      - ./env/mysql.env
  composer: # package manager for PHP. this container used internally by Laravel.
    # image: 'composer:latest'
    build:
      context: ./dockerfiles
      dockerfile: composer.dockerfile
    volumes:
      - ./src:/var/www/html # 소스코드를 컨테이너에 노출. composer을 사용하여 컨테이너 내부의 이 폴더에서 Laravel 앱을 생성하면 로컬 머신의 소스 폴더로 미러링됨
  artisan: # db migrate
    build:
      context: .
      dockerfile: dockerfiles/php.dockerfile
    volumes:
      - ./src:/var/www/html
    entrypoint: ['php', '/var/www/html/artisan']
  npm: # package manager for front-end
    image: node:14
    working_dir: /var/www/html
    entrypoint: ['npm']
    volumes:
      - ./src:/var/www/html
```

## Dockerfile 

### PHP dockerfile
```dockerfile
# Nginx 구성을 위해 php-fpm 이미지 필요
FROM php:8.0-fpm-alpine 
 
# 웹 서버 표준적인 폴더. 최종 Laravel PHP 애플리케이션을 보관해야 하는 컨테이너 내부의 폴더
WORKDIR /var/www/html
 
COPY src .

# php 확장 프로그램 설치
RUN docker-php-ext-install pdo pdo_mysql
 
RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

USER laravel 
 
# RUN chown -R laravel:laravel .

# 끝에 CMD 또는 ENTRYPOINT가 없으면 베이스 이미지의 CMD 또는 ENTRYPOINT를 사용한다.
```

### composer dockerfile
```dockerfile
FROM composer:latest
 
RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

USER laravel 

# 코드가 있을 곳
WORKDIR /var/www/html 
 
# composer 실행, 일부 종속성이 누락되어도 경고나 오류없이 실행할 수 있게 해주는 역할
ENTRYPOINT [ "composer", "--ignore-platform-reqs" ]
```

### nginx dockerfile
```dockerfile
FROM nginx:stable-alpine
 
WORKDIR /etc/nginx/conf.d
 
COPY nginx/nginx.conf .
 
# nginx 가 예상하는 conf 파일이름이 default.conf 라서 파일 이름을 바꿔줌
RUN mv nginx.conf default.conf 

WORKDIR /var/www/html
 
COPY src .

# nginx 자체 이미지 내부에 디폴트 명령어가 있기 때문에 CMD / ENTRYPOINT 사용할 필요 없음
```


### command
```bash
# docker-compose로 단일 컨테이너 실행 + laravel create project
# bind mount 때문에 명령어가 끝나면 /src에 Laravel 프로젝트 파일들이 생긴다.
docker-compose run --rm composer create-project --prefer-dist laravel/laravel:8.0.0 .

docker-compose build server php mysql

docker-compose up -d server php mysql

# composer 를 제외하고 다른 컨테이너들 실행
docker-compose up server php mysql

# docker-compose 가 Dockerfile을 보고 이미지를 리빌드하게 할 때
docker-compose up -d --build server

# db migration
docker-compose run --rm artisan migrate

```