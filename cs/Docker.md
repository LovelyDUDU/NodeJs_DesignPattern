# Docker

![docker](../../img/docker.png)

## Docker란?

Docker는 **컨테이너 기반의 오픈소스 가상화 플랫폼** 이다.

여기서 말하는 컨테이너는 분리된 작업공간(독립된 업무공간 구축)을 뜻한다.

## Docker를 왜 쓰는가?

성능 향상, 뛰어난 이식성, 유연성 때문에 쓴다. 쉽게 말해 서버를 관리하고 서비스들을 배포하는 일을 쉽게하기 위해 사용한다.

> **성능 향상과 유연성** : 컨테이너를 이용하면 자원낭비를 줄일 수 있고, 실행 환경을 독립적으로 구성해주어 버전이 달라서 생기는 문제가 일어나지 않는다. 
> 
> **뛰어난 이식성** : 개발 컴퓨터와 서버 컴퓨터에 환경을 동일하게 맞출때 용이하다.

예를 들어 서비스A와 서비스B가 있다고 치자.

서비스A는 React(+Node.js), TypeScript(+Node.js), MySQL 로 이루어져 있다.

서비스B는 Vue(+Node.js), JavaScript(+Node.js), MySQL 로 이루어져 있다.

보통 개발 작업은 개발자의 컴퓨터에서 코딩을 하고, 테스트를 진행해서 별 이상이 없으면 그것들을서버에 올려서 사람들이 볼 수 있도록 서버에서 프로그램을 실행한다. 우리는 이러한 작업을 '배포' 라고 한다.

배포까지의 작업을 위해서는 개발자의 컴퓨터와 서버 컴퓨터 모두에 서비스A에 사용될 수 있는 React(+Node.js), TypeScript(+Node.js), MySQL 버전들이 동일하게 깔려있어야 한다.

그런데 개발자의 컴퓨터와 서버 컴퓨터 모두에 일일이 버전을 동일하게 맟줘주는 것은 귀찮고, 실수의 가능성도 있다.

개발자의 컴퓨터와 서버 컴퓨터에 환경을 세팅해줄때 뿐만 아니라, 서버를 운영하다가 더 좋은 서버로 옮기거나, 서버를 여럿 추가해야하는 경우가 생길 수도 있다.

그리고 같은 서버에 서비스A와 서비스B를 동시에 돌리는 경우, 각각이 다른 실행환경에서 동작해야 할 때 일이 까다로워 질 수 있다.

예를 들어 서비스A는 Node.js 버전이 14이상이여야 하고, 서비스B는 10인 경우에 이것저것 신경 써줘야한다.

이러한 불편함을 해소해주는 것이 Docker다.

## Dokcer는 어떻게 사용되는가?

Docker는 각 요소들이 설치된 모습을 '이미지'라는 형태로 박제해서 저장한다. <br>*이미지 = 컨테이너를 찍어내는 틀 또는 무한 생성 가능한 컨테이너 조립키트*

각 제품마다 공식적으로 제공되는 이미지도 있고, 사용자가 원하는대로 만들어낼 수도 있다. git으로 저장된 내용이 github에 올려지는 것처럼, 이런 도커 이미지들은 **DockerHub** 라는 곳에 업로드되서
공유되고 다운이 가능하다.

이렇게 이미지로 저장된 항목들이 함께 연결되서 동작하도록 설정된 상태를 명령어 텍스트나 문서 형태로 저장할 수도 있다.

마치 이게 설치되는 과정을 어디서든 컴퓨터가 자동으로 재현할 수 있도록 녹화해둔다는 느낌으로 보면 된다.

이 문서만 잘 보관해두면 이 요소들이 언제 어디서든 미리 지정된, 서비스에 필요한 설정대로 DockerHub로부터 받아져서 설치될 수 있다.

이렇게 컴퓨터가 자동으로 재현해준다면 위에서 얘기했던 버전실수나 귀찮음이 해소된다.

하지만 이때 docker는 이것들을 컴퓨터에 바로 설치는 하지 않는다.

각각을 **컨테이너** 라고 불리는 독립된 가상 공간을 만들어내서 복원한다. 그렇기 때문에 위에서 서비스A와 서비스B가 Node.js버전이 달라서 생기는 문제들이 해소된다. 각각의 컨테이너 안에서 서로 방해받는 일
없이 돌아가기 때문이다.

## VirtualBox같은 가상 컴퓨팅과 Docker의 차이

가상 컴퓨팅은 한 물리적 컴퓨터 안에 각각 OS를 가동하는 가상 컴퓨터들이 물리적 자원을 분할해서 쓰기 때문에 성능에 한계가 있다.

![VirtualBox](../../img/VirtualBox.JPG)

하나의 컴퓨터를 집으로 비유해서 생각을 해보자. 한 컴퓨터 안에 각각의 OS를 구동한다는 것은 집안에 또다른 집을 짓는다는 것이고, 물리적 자원을 분할한다는 것은 전기과 수도를 각 집집마다 정해진 비율에
맞게 나눠서 사용한다는 것이다. 사용할 수 있는 에너지가 집집마다 정해져있다는 것이다. 그리고 OS기능들(집으로 예시를 들자면 화장실이나 부엌)들이 각 집마다 또 들어간다. 이는 매우 비효율적이다.

반면 Docker는 OS단까지 내려가는게 아니라 실행 환경만 독립적으로 돌리는거라서 컴퓨터를 직접 요소들을 설치한거랑 별 차이없는 성능을 낼 수 있고, 가상 컴퓨팅보다 훨씬 가볍고 빠르게 각각을 설치하고 실행하고
켜고 끄고 서로 연동이 가능하다.

![VirtualBox](../../img/DockerInner.JPG)

Docker는 집안에 업무공간으로만 사용하는 컨테이너들이 들어간다. 한 컨테이너안에 여러 프로그램(Node.js, React, MySQL)이 들어갈 수도 있고, 각각 컨테이너에 들어가서 서비스가 돌아갈 수도 있다.
이 컨테이너들은 집에서 딱 필요한 공간만을 차지하고, 전기와 수도도 공유해서 사용하기 때문에 집에 집이 들어가서 자원이 낭비되는 것도 적고, 각각의 업무공간이 확실히 분리되기 때문에 버전이 달라도 문제가 없다. 

Docker같은 경우엔 서버에 뭐가 잘못돼서 고쳐야 하거나, 일부를 업그레이드해야 하거나 할 때는 일일이 요소들을 정지하고 지우거나 새로 깔거나 할 필요없이 그냥 이 컨테이너들을 통째로 교체해서 새로 실행하면 됨.



<hr>

## Docker 명령어 ( https://www.yalco.kr/36_docker/ )

`docker run -it node`

run : 이미지가 없으면 다운받은 다음 그 이미지를 내 컴퓨터에서 해동해서 컨테이너로 만듬

-it : 컨테이너를 연 다음, 그 환경 안에서 CLI를 사용한다는 것.

==> 컨테이너가 만들어져 설치되면 바로 node 명령어가 실행되도록 설계되어 있음.

`docker images`

=> docker 이미지들을 조회함

`docker ps`

=> 현재 실행중인 docker 컨테이너의 리스트를 반환해주는 명령어 (모든 컨테이너를 보려면 -a 옵션 추가)

`docker exec -it (컨테이너명) bash`

=> (컨테이너명) 안에서 bash shell을 실행한다는 명령어

윈도우에서 파워쉘로 CLI명령어를 입력할 수 있는 것과 같음.


## Docker 파일 정리

Dockerfile : 나만의 이미지를 만들기 위한 설계도

컴퓨터에서 http-server란 프로그램을 돌리려면 Node.js 만설치되어 있어서는 안됨.

npm 등으로 http-server가 전역으로 깔려 있어야 함.

node 이미지는 아직 http-server는 없는 상태이기 떄문에 이를 컨테이너로 실행할 때마다 매번 이걸(http-server) 받아줘야 함.

서비스를 돌릴 때 마다 이런 모듈들을 일일이 다운받아야 한다는 문제가 있음.

==> DockerHub에서 받아온 이미지에는 Node.js만 있음. 
그런데 http-server라는 프로그램을 돌리려면 Node.js 만 설치되어있으면 안됨.
그래서 Node.js에다가 http-server까지 깔린 상태가 이미지로 있으면 컨테이너로 실행할 때마다 모듈을 다운받아올 필요가 없어져서 컨테이너로 서비스를 실행하기가 보다 수월해짐.

## Dockerfile 내에서 사용되는 명령어

`RUN`

이미지 생성 과정에서 실행할 명령어

`WORKDIR`

이미지 내에서 명려어를 실행 할(현 위치로 잡음) 디렉토리 설정

`CMD`

컨테이너 실행시 실행할 명령어


















<!-- 메모
<hr>

이 서비스A를 한마디로 정의 하자면예시를 들면서 이해를 하면 쉽다.

참고 -> https://www.youtube.com/watch?v=tPjpcsgxgWc

분리된 작업공간(독립된 업무공간 구축) => Container. Docker만 있으면 docker hub에 업로드한 장비들을 내가 지정한 형태한 형태로 어디든 설치할 수 있고, 장비들에 문제가 생기더라도 그냥 새로

복원이 가능.

같은 곳의 다른 비품들과 서로 방해하지 않는 해당 작업만의 업무환경을 만들어낼 수 있음.

서버를 돌리기 위한 환경을 구축하는데 도움이 됨.

언어, 웹서버, DB, 자동배포툴 등 여러가지를 버전 신경써서 다운받은 다음, 서로 잘 맞물려 동작할 수 있도록 이것저것 설정해야함.

근데 서버를 운영하다보면 더 성능 좋은 서버로 옮겨가거나, 늘어난 접속량 처리를 위해 서버를 여럿 추가해야 하게 될 수 있음..

그러면 또 일일이 다 설치해줘야함. 이걸 한명이 하면 그나마 나은데 그게 아닐 가능성이 높으니깐...

뿐만 아니라, 같은 서버에 여러 서비스를 돌리는 경우, 각각이 다른 실행환경에서 동작해야 할 때 일이 까다로워 질 수 있음.

예를들어 기존서버는 java7이었는데 새서비스가 java8에서 동작하는거면 또 이것저것 신경써줘야함.

먼저 각 요소들이 설치된 모습을 이미지라는 형태로 박제해서 저장함. 각 제품마다 공식적으로 제공되는 이미지도 있고,

사용자가 원하는대로 만들어낼 수 도 있음.

git으로 저장된 내용이 github에 올려지는 것처럼, 이 도커 이미지들은 Dockerhub라는 곳에 업로드되서 공유되고 다운 가능.

그리고 이렇게 이미지로 저장된 항목들이 함께 연결되서 동작하도록 설정된 상태를 명령어 텍스트나 문서 형태로 저장할 수도 있음.

마치 이게 설치되는 과정을 어디서든 컴퓨터가 자동으로 재현할 수 있도록 녹화해둔다는 느낌으로!

이 문서만 잘 보관해두면 이 요소들이 언제 어디서든 미리 지정된, 서비스에 필요한 설정대로 DockerHub로부터 받아져서 설치될 수 있음.

docker는 이것들을 컴퓨터에 바로 설치는 안함.

각각을 컨테이너라고 불리는 독립된 가상 공간을 만들어내서 복원함.

때문에 아까 말했던 서비스 처럼 다른 버전의 자바를 돌리는 서비스들도 각각의 컨테이너 안에서 서로 방해받는 일 없이 돌아갈 수 있음.

virtualBox같은 가상 컴퓨팅하고는 다른 구조.

가상 컴퓨팅은 한 물리적 컴퓨터 안에 각각 OS를 가동하는 가상 컴퓨터들이 물리적 자원을 분할해서 쓰기 때문에 성능에 한계가 있음.

![VirtualBox](../../img/VirtualBox.JPG)

도커는 OS단까지 내려가는게 아니라 실행 환경만 독립적으로 돌리는거라서 컴퓨터를 직접 요소들을 설치한거랑 별 차이없는 성능을 낼 수 있고, 가상 컴퓨팅보다 훨씬 가볍고 빠르게 각각을 설치하고 실행하고 켜고 끄고 서로

연동이 가능하다.

![VirtualBox](../../img/DockerInner.JPG)

그래서 이제는 서버에 뭐가 잘못돼서 고쳐야 하거나, 일부를 업그레이드해야 하거나 할 때는 일일이 요소들을 정지하고 지우거나 새로 깔거나 할 필요없이 그냥 이 컨테이너들을 통째로 교체해서 새로 실행하면 됨.

서버를 관리하고 서비스들을 배포하는 일이 쉬워진거임.

<hr>

참고 -> https://www.youtube.com/watch?v=hWPv9LMlme8

기본적으로 Front-end, Back-end, DB로 구성되는 서비스를 예시로 들겠다.

Front-end -> Svelte로 짜서 build한 결과물을 Node.js의 http-server에 올릴꺼임. html, css, js를 브라우저에서 사이트로 볼 수 있도록 웹서버를 돌릴꺼임.

Back-end -> python으로 된 Flask 프레임워크를 사용함. rest api로 데이터를 프론트와 주고받을꺼임. 여기에 사용될 데이터들은 DB에 저장됨

DB -> MySQL 을 사용

개발 작업은 보통 개발자의 컴퓨터에서 코딩을 하고, 테스트를 진행해서 별 이상이 없으면 그것들을 서버에 올려서 사람들이 볼 수 있도록 서버에서 프로그램을 실행, 즉 '배포'를 해야함.

그러려면 개발자의 컴퓨터와 서버 모두에 이를 돌릴수 있는 Node.js, Python, MySQL이 가능한 버전들이 동일하게 깔려있어야함.

개발용 컴퓨터, 서버용 컴퓨터
-->
