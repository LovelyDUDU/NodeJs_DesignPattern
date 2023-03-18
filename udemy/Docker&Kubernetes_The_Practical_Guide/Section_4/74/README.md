# 74. Docker Networks 소개: 우아한 컨테이너 간 통신

도커로 컨테이너 네트워크 라는 것을 만들 수 있다.

여기서 말하는 네트워크는 컨테이너가 여러개 있을 때 (다중 컨테이너 일 때) 컨테이너 간의 통신을 허용하는 것이다.

특정 네트워크를 만든 다음 해당 네트워크에 컨테이너를 포함시킬 때는 `--network` 옵션을 컨테이너를 실행시킬 때 사용하면 된다.

컨테이너 네트워크를 사용하면 이전에 수동으로 했었던 IP 조회 및 해결 작업을 자동으로 할 수 있다.

네트워크는 볼륨과 처럼 도커가 자동으로 생성해주지 않기 때문에 직접 명령어로 만들어줘야 한다. 

```bash
# favorites-net 이라는 네트워크를 만들겠다.
docker network create favorites-net

# docker network 조회
docker network ls

# 몇 가지 내장된 디폴트 네트워크도 있다.
NETWORK ID     NAME            DRIVER    SCOPE
845bbb664111   bridge          bridge    local
9e361ab35764   favorites-net   bridge    local
bc8b689402cc   host            host      local
6a01ccc03631   none            null      local


```


```bash
# db connection 부분 코드를 수정하고 이미지 새로 만들기
docker build -t favorites-node .

# mongodb 라는 이름의 컨테이너를 만들고 네트워크 옵션을 추가해주었다. 
docker run -d --name mongodb --network favorites-net mongo

# 새로 만든 이미지를 가지고 컨테이너 실행
docker run --name favorites --network favorites-net -d --rm -p 3000:3000 favorites-node
```

mongodb 컨테이너를 생성한 후에 `app.js` 에서 db에 연결하는 부분의 코드를 수정해줘야 한다. `172.17.0.2:27017` -> `mongodb:27017` 로 바꿔주었는데 여기서 말하는 `mongodb` 는 mongo db의 컨테이너 이름이다. (`--name mongodb`)
컨테이너 네트워크를 사용하면 컨테이너 이름을 도메인(주소) 로 사용할 수 있다.

네트워크에 컨테이너를 포함시킬 때는 포트를 별도로 지정해주지 않아도 된다. `-p` 옵션은 로컬 호스트 머신이나 컨테이너 네트워크 외부에서 그 컨테이너의 무언가에 연결할 계획인 경우만 필요한데 지금과 같이 동일한 도커 네트워크 안에서 컨테이너 간에 연결을 할 때는 포트를 노출할 필요가 없다. (모든 컨테이너가 동일한 네트워크 내부에서 자유롭게 통신할 수 있기 때문)



