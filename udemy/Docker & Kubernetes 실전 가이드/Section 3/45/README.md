# 45. 데모 앱 구축 & 이해하기

```
docker build -t "feedback-node" .

# -d: detach,
# --name <name>: set container name
# --rm : remove container when container stop
docker run -p 3000:80 -d --name feedback-app --rm feedback-node
```
실행된 어플리케이션에서 텍스트를 입력하고 저장된 파일은 도커의 컨테이너 안에 내장되어있을뿐, 로컬 머신의 `/feedback` 에는 영향을 주지 않는다.

컨테이너는 격리되어야 한다. 

이미지를 생성하는데 사용했던 스냅샷이 복사되니 격리된 파일 시스템을 가지게 된다.

