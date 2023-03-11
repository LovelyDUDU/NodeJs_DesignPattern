# 95. Docker Compose Up과 Down
`docker-compose up` 명령어는 attached 모드로 빌드가 실행된다. detached 모드로 실행하고 싶으면 `-d` 옵션을 추가해야한다. 

`docker-compose down` 은 서비스를 종료할 때 사용한다. 모든 컨테이너가 삭제되고 생성된 디폴트 네트워크와 모든 것이 삭제되는데 볼륨은 삭제되지 않는다. 볼륨도 삭제하려면 `-v` 옵션을 추가하면 되는데 일반적으로 잘 사용하지 않는다.