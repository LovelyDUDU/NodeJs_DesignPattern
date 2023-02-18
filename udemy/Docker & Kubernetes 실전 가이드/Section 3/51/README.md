# 51. 바인드 마운트 시작하기(코드 공유)

바인드 마운트(Bind Mount)는 Named Volume과 달리 호스트 머신의 파일 시스템 상의 볼륨 위치를 정할 수 있다. 

바인드 마운트는 영구적이고 편집 가능한 데이터에 적합하다.

바인드 마운트 설정은 Dockerfile과는 관련이 없고 conatiner를 실행할때 옵션으로 설정해줘야 한다. (이미지에 영향을 주는 것이 아니라)

`<local path>:<volume path>` 에서 local Path 파일 전체를 volume path에 매핑하는 것이다.

예를들어 `/Users/Download:/app`  일 경우 `/Users/Download` 에 있는 모든 파일이 `/app` 에 매핑된다는 뜻이다.

```bash
# docker run -d -p 3000:80 --rm --name <container name> -v <볼륨 이름>:<컨테이너 내부 볼륨 위치> -v "<local path>:<volume path>"

docker run -d -p 3000:80 --rm --name feedback-node -v feedback:/app/feedback -v $(pwd):/app feedback-node:latest
```