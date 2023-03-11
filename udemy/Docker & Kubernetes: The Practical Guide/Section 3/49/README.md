# 49. 명명된(named) 볼륨으로 구조하기!

도커에는 2가지 외부 데이터 저장 메커니즘이 있다.

하나는 볼륨(Volume)이고, 나머지 하나는 마운트(Mount) 이다

볼륨에는 익명(anonymous)볼륨과 명명(named)볼륨이 있는데, 

Dockerfile에서 명령어로 추가하는 `VOLUMES ["/app/feedback"] 은 익명 볼륨이다.

익명 볼륨은 컨테이너가 존재하는 동안에만 실제로 존재한다.

그래서 삭제되지 않는 볼륨이 필요할 때는 명명된(named)볼륨을 사용해야한다.

named volume은 Dockerfiile이 아닌 컨테이너를 생성할때 `-v <volume name>:<volume path>` 명령어를 추가하여 생성한다.

생성된 named volume에 저장된 데이터는 컨테이너가 삭제되어도 남아있다.