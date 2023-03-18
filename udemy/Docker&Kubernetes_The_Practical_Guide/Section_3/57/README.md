# 57. Docker 볼륨 관리하기

`docker volume ls` 로 도커가 관리중인 모든 볼륨을 볼 수 있다.

물론 바인드 마운트는 도커가 관리하는 볼륨이 아니기 때문에 리스트에 표시되지 않는다.

```bash
>> docker volume ls
DRIVER    VOLUME NAME
local     0baf15997e97d6fda0028ab07826ecbe1cfa0d0ae0c188f66bad027c51a4f1e9
local     a28267b3e1971983c95292762ad13549b6f83924121c1e484aa52d092e3933ff
local     feedback
```

feedback은 56장에서 만들었던 named volume이고, 나머지 두개는 `/app/temp` 와 `app/node_modules` 로 익명볼륨이다.

`docker volume create <volume name>` 를 사용하여 named 볼륨을 만들 수도 있다. 

`docker volume instace <volume name>` 을 사용하면 볼륨에 대한 정보도 알 수 있다. 

```bash
DOCKER-COMPLETE % docker volume inspect 0baf15997e97d6fda0028ab07826ecbe1cfa0d0ae0c188f66bad027c51a4f1e9
[
    {
        "CreatedAt": "2023-02-22T14:37:54Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/0baf15997e97d6fda0028ab07826ecbe1cfa0d0ae0c188f66bad027c51a4f1e9/_data",
        "Name": "0baf15997e97d6fda0028ab07826ecbe1cfa0d0ae0c188f66bad027c51a4f1e9",
        "Options": null,
        "Scope": "local"
    }
]
```

이때 나오는 Mountpoint는 실제로 데이터가 저장되는 호스트 머신상의 경로인데, 로컬에 있는 경로가 아니다. 여기서 말하는 경로는 도커가 설정한 가상 머신 내부이기 때문이다. (도커가 실행되는 기본 가상 머신이 있는경우, 이 경로가 그 머신의 경로라고 할 수 있다.)

