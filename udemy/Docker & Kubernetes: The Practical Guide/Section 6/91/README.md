# 91. Docker-Compose: 무엇이며 왜 사용하는가?

`Docker-Compose`란?

`docker build` 와 `docker run` 명령을 대체할 수 있는 도구이다. 이는 모든 서비스와 모든 컨테이너를 즉시 시작하고 필요하다면 모든 필요한 이미지를 빌드하는 오케스트레이션 명령 셋이다. (자동화된 설정)

- Docker-Compose는 `Dockerfile`을 대체하지 않는다.
- Docker-Compose는 이미지나 컨테이너를 대체하지 않는다.
- Docker-Compose는 다수의 호스트에서 다중 컨테이너를 관리하는데 적합하지 않다.
- Docker-Compose는 하나의 동일한 호스트에서 다중 컨테이너를 관리하는데 좋다.

compose 파일에서는 포트와 환경변수 그리고 볼륨을 정의할 수 있고 네트워크를 할당할 수 있다. (이전에 했던 모든 옵션들을 정의하고 설정할 수 있다)