# 30. 이미 실행 중인 컨테이너에 연결하기

default로 `-d` 없이 컨테이너를 실행하면 "attached모드" 로 실행이 된다.

detached모드(`-d`)로 컨테이너를 실행한 경우에는 

`docker container attach <container name>`

명령어를 통해 컨테이너를 재시작하지 않고 컨테이너에 연결할 수 있다.