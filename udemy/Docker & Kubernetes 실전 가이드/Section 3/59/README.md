# 59. 모든 것을 복사하진 마세요: "dockerignore" 파일 사용하기

`.dockerignore`는 Dockerfile의 `COPY` 명령어에서 모든 것이 복사되는 것을 막아준다.

`.gitignore` 과 비슷하다고 생각하면 된다.

(`.gitignore`은 git에 커밋되는 것을 막아주고, `.dockerignore`은 COPY 명령으로 복사되는 것을 막아준다.)

예를들어 `node_modules`를 추가하면 호스트 머신의 프로젝트 폴더에서 `node_modules` 폴더가 이미지에 복사되지 않게 막아준다.