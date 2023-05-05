# 133. 앱 실행 & 게시하기 (EC2에서)

```bash
# docker 이미지 실행
sudo docker run -d --rm -p 80:80 harry/node-example-1
```

Mac M1 일 경우 이미지를 생성할때 `--platform linux/amd64` 옵션을 넣었어야 정상적으로 컨테이너가 실행된다.


보안그룹
- 아웃바운드: 다른 곳에 있는 인스턴스 대기열로부터 허용되는 트래픽을 제어 (들어오는거 설정)
- 인바운드: 나가는거 설정

ec2 인바운드 규칙은 기본적으로 다 막혀있기 때문에 규칙을 추가해줘야함.

HTTP - Anywhere