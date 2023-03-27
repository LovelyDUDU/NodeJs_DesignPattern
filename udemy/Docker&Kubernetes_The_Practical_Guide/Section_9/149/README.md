# 149. 프로덕션에서 MongoDB Atlas 사용하기
Production 환경에서 MongoDB Atlas 를 사용하기 위해서는 우선 mongo DB 컨테이너 및 EFS 를 제거해야 한다.

그 다음 Task Definition에서 goals-backend 구성의 mongo db 연결 부분에 URL 로 입력했던 localhost 대신 Cluster URL을 입력하면 된다.