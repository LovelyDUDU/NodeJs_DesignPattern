# Section 1

## 내용

### Docker 란?

컨테이너를 생성하고 관리하기 위한 도구이자 기술.

</br>

### Container 란?
표준화된 소프트웨어 유닛으로 하나의 코드 패키지이다. 해당 코드를 실행하는데 필요한 dependencies 와 tools 가 포함되어 있다.

</br>

### 독립적이고 표준화된 application package 가 필요한 이유
1. Different Development & Production Environments
  
   코드가 항상 정확한 버전으로 실행되도록 할 수 있다. 개발환경과 배포환경을 동일하게 맞춰줄 수 있다.

2. Different Development Environments Within a Team / Company

    A의 개발환경과 B의 개발환경을 동일하게 맞춰줄 수 있다.
3. Clashing Tools / Versions Between Different Projects

    작업중인 프로젝트가 여러 개인 경우 충돌하는 버전이 있을 수 있는데 도커를 사용하면 프로젝트간의 버전 충돌이 일어나지 않는다 (ex, nodejs 14 or 12)

</br>

### Docker 와 Container가 아닌 VM을 이용한다면?
도커와 컨테이너를 사용하는 것과 동일한 구현은 가능하다. 하지만 VM은 컴퓨터 안에 컴퓨터를 설치하는 것이라 memory, cpu, capacity 등 자원 낭비가 심하다. 

</br>

### Docker 설치 (mac)
https://www.docker.com/

brew install --cask docker

</br>

### Daemon
계속 실행되면서 도커가 작동하는지 확인하는 프로세스

</br>

### Docker Compose
Docker를 기반으로 하는 도구로, 복잡한 컨테이너 또는 다중 컨테이너 프로젝트를 쉽게 관리할 수 있게 해준다.

</br>

### Kubernetes
복잡하게 컨테이너화된 애플리케이션을 배포할 때, 이를 관리하는데 도움이 됨


</br>

### Dockerfile 을 이용하여 이미지 빌드
```
docker build .
```

그리고 생성된 컨테이너를 이용하여 run

```
docker run -p 3000:3000 sha256:23f97a10d9baf3d672c43e89a0b6bc513dcd004611
```

만약 
```
Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
```
이런 에러가 발생했다면 docker scan을 해줘야하는데 그 전에 docker 회원가입 및 로그인을 해야한다.


<br><hr>

## 정리

### 용어 정리

* **Docker** :  컨테이너를 생성하고 관리하기 위한 도구(툴)이자 기술이다.
* **Image** : 앱이 실행되는 환경까지 감싸진 일종의 코드 패키지이자 하나의 코드 덩어리이다. 
* **Container** : 이미지로 만든 배포 인스턴스이다.
* 이미지는 치킨레시피 이고, Container는 치킨 레시피로 만든 치킨이다.
* **Kubernetes** : 컨테이너를 관리하는 도구를 이용하여 컨테이너를 관리하는 도구(툴)이다.

### Dockerfile을 이용하여 이미지를 빌드할때

```bash
# Dockerfile이 있는 위치에서 
docker build .
```

### 빌드된 이미지를 이용하여 컨테이너를 생성 및 실행할 때

```bash
# 첫번째 3000은 로컬머신의 포트, 두번째 3000은 컨테이너에서 외부로 개방할 포트 
docker run -p 3000:3000 <img name>
```