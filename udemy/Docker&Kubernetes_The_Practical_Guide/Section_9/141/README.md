# 141. 다중 컨테이너 앱 준비하기

컴포즈 파일은 동일한 머신에서 다중 컨테이너를 관리하고 실행하는데 중요하다. 대신 여러 대의 머신이 함께 작동할 클라우드로 이동하기 시작하는 순간 한계에 도달한다.

AWS ECS에서는 컨테이너 IP 자동찾기 기능을 사용할 수 없다. (컨테이너가 항상 동일한 머신에서 실행될 가능성이 거의 없기 때문에) 

대신 동일한 태스크에 컨테이너를 추가하면 동일한 머신에서의 실행이 보장된다. 그래도 ECS는 도커 네트워크를 생성하지 않고, 대신 localhost를 컨테이너 애플리케이션 코드 내부의 주소로 사용할 수 있게 해준다.

```
# mongodb = 연결하고자 하는 컨테이너의 이름
MONGODB_URL=mongodb
```

```
docker build -t goals-node --platform linux/amd64 ./backend/

docker tag goals-node seunghwankim/goals-node

docker push seunghwankim/goals-node
```