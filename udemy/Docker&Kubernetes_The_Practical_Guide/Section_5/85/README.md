# 85. 볼륨으로 MongoDB에 데이터 지속성 추가하기
84 장에서 생성한 mongodb 컨테이너는 중지되었다가 새로 컨테이너를 만들었을때 데이터가 날아간다. `-v` 옵션을 사용하여 볼륨을 사용하면  데이터를 유지할 수 있다.

mongodb 컨테이너가 db 데이터를 저장하기 위해 내부적으로 사용하는 경로는 mongo 컨테이너 문서에서 확인할 수 있다. 

`https://hub.docker.com/_/mongo`

```bash
# MongoDB가 컨테이너 내부에 db 파일을 저장하는 곳은 /data/db 폴더 이다.
# /my/own/datadir:/data/db -> 바인드 마운트에 매핑

$ docker run --name some-mongo -v /my/own/datadir:/data/db -d mongo
```

```bash
# `-v` 옵션을 이용하여 명명된 볼륨을 사용
docker run --name mongodb -v data:/data/db --rm -d --network goals-net mongo
```

`MONGO_INITDB_ROOT_USERNAME`, `MONGO_INITDB_ROOT_PASSWORD` : MongoDB 이미지에서 지원하는 db 접근 방지 환경변수. 이 환경변수를 사용하면 db에 액세스할 때 사용자 이름과 비밀번호를 필요로 하게 된다.

```bash
docker run --name mongodb -v data:/data/db --rm -d --network goals-net -e MONGO_INITDB_ROOT_USERNAME=max -e MONGO_INITDB_ROOT_PASSWORD=secret mongo
```

backend 소스코드도 수정해야 한다. 호스트 주소 앞에 `사용자 이름:비밀번호@` 형식을 추가하고, 문자열 끝에 `authSource=admin` 쿼리 매개변수를 추가해야 한다.
```javascript
// "mongodb://mongodb:27017/course-goals",
"mongodb://max:secret@mongodb:27017/course-goals?authSource=admin",
```