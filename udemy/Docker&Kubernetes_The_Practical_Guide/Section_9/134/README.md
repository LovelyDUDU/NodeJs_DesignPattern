# 134. 컨테이너/이미지 관리 & 업데이트

### Remote 서버에 코드 업데이트를 push 하는 법
이미지를 재빌드하고 업데이트된 이미지를 사용하면 된다. 재빌드하고 해당 이미지를 push하면 실제로 변경된 부분에 대해서만 push하고 변경되지 않은 레이어는 재사용한다. (레이어 기반 아키택쳐ㄴ)
```bash
# 재빌드
docker build -t node-dep-example-1 --platform linux/amd64 .

docker tag node-dep-example-1 seunghwankim/node-example-1

docker push seunghwankim/node-example-1
```

```
sudo docker run -d --rm -p 80:80 seunghwankim/node-example-1
```

근데 실제로 리로드하면 수정전 버전이 화면에 나옴. 왜냐하면 도커가 이미지를 실행할 때마다 그 이미지가 로컬에 있는지 여부를 확인하기 때문.

로컬에 찾고자 하는 이미지가 있으면 그걸 그대로 쓰기 때문에 `docker pull` 로 받아와서 컨테이너 시작해줘야함.

```
sudo docker pull seunghwankim/node-example-1

sudo docker run -d --rm -p 80:80 seunghwankim/node-example-1
```


### 중지하고 종료하는법
웹서버 종료는 docker stop 

인스턴스 종료는 aws 에서 terminate instance