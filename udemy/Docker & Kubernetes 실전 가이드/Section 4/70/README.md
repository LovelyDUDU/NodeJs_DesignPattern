# 70. 데모 앱 분석하기

`/70/DOCKER-COMPLETE` 에 있는 데모앱은

4개의 엔드 포인트가 있으며, 다음과 같은 기능에 대해서 살펴볼 예정이다.

- `/movies` 에 대한 GET (더미 API에 데이터 호출)
- `/people` 에 대한 GET (더미 API에 데이터 호출)
- `/favorites` 에 대한 GET, POST (다른 컨테이너 혹은 호스트 머신에 있는 DB로 부터의 통신)

