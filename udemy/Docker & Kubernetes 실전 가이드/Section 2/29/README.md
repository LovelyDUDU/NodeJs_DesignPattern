# 29. Attached & Detached컨테이너 이해하기

`docker start` 를 하면 `docker run` 처럼 터미널이 fore ground로 실행되지 않고 컨테이너가 백그라운드로 실행된다. 

`docker start` 는 detached(분리) 모드가 기본이고, `docker run` 은 attached (연결) 모드가 기본이라서 그렇다.

attached 일때는 컨테이너와 연결된 상태이기 때문에 console.log 가 터미널에 출력된다. 즉 attached 모드는 단순히 컨테이너의 출력 결과를 수신한다는 것을 의미한다.

`docker run` 명령어에서도 `-d` 옵션을 추가하면 detached(백그라운드) 모드로 컨테이너를 실행시킬 수 있다. 

컨테이너를 다시 attach 모드로 변경하려면 `docker container attach <conatiner name>` 로 바꿀 수 있다. (실행중인 컨테이너에 다시 attach 모드로 연결하는 것이다.)

`docker logs` 명령어를 사용하면 컨테이너에 출력된 과거 로그를 가져올 수 있다.

`docker logs` 명령어에 `-f` 옵션을 추가하면 follow 모드로 진입하여 로그정보를 실시간으로 받아볼 수 있게된다. `docker logs <container id>`

중지된 컨테이너를 재시작하고 싶다면 `docker start -a <container name>` 를 사용하여 attached 모드로 바로 시작할 수 있다.