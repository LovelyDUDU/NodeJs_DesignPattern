# 56. 읽기 전용 볼륨 살펴보기

컨테이너가 'app' 폴더에 쓸 수 있어야 하는 것이 아니라, 컨테이너가 로컬에 있는 파일을 변경할 수 없어야 하는게 옳다. (파일을 변경할 수 있는 곳은 호스트 머신 파일 시스템이지 컨테이너가 아니니깐.)

이럴때 바인드 마운트를 읽기 전용(read-only)로 전환하면 된다. (볼륨의 디폴트는 read-write이다.)

ro(read-only)로 볼륨을 만들 때는 내부 경로 뒤에 `:ro` 를 추가하면 된다.

```
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "$(pwd):/app:ro" -v /app/node_modules -v /app/temp feedback-node:volumes
b9706cb70b795aba73f1431e14e86eb1534988ebccfb21526adcf5a45e5bd05b
```