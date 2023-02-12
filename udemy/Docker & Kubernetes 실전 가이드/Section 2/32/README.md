# 32. 이미지 & 컨테이너 삭제하기

`docker rm <container id>` 는 중지된 컨테이너 삭제할때 사용한다. 실행중인 컨테이너를 `rm` 으로 제거하려고 하면 에러가 발생한다.

여러 중지된 컨테이너를 삭제하고 싶을땐 `<container id>` 를 여러개 입력해도 된다.

`docker container prune` 명령어를 사용하면 중지된 모든 컨테이너를 한꺼번에 삭제할 수도 있다.

`docker images` 명령어는 image를 리스팅한다.

도커 이미지를 삭제할때는 `docker rmi` 명령어를 사용한다. 이미지를 삭제하게 되면 해당 이미지 내부의 모든 레이어를 함꼐 삭제한다. 이미지가 더이상 컨테이너에서 사용되지 않고 중지된 컨테이너에 포함된 경우에만 이미지를 제거할 수 있다. (이미지에 해당하는 컨테이너가 남아있다면 해당 이미지를 삭제할 수 없다는 뜻이다.)

`docker image prune` 는 이미지를 한꺼번에 삭제할 때 사용한다.

## 정리

###  컨테이너

```bash
# 중지된 컨테이너 삭제
docker rm <container id>

# 중지된 컨테이너 모두 삭제
docker container prune
```

### 이미지 

```bash
# 이미지 리스트 출력
docker images

# 이미지 삭제 (이미지에 해당하는 컨테이너가 없을때 만 삭제가 가능하다)
docker rmi <image id>

# 이미지 모두 삭제
docker images prune
```