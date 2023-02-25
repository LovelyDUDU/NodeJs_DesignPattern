# 71. 컨테이너 만들기 & 웹 통신하기(WWW)

app.js 파일에서 DB와 연결하는 부분은 우선 주석처리를 임시로 한다. (아직 DB 컨테이너가 없기 때문)

기본적으로 컨테이너는 www에 요청을 보낼 수 있다. 도커화된 애플리케이션 내부에서 특별한 설정이나 코드 변경없이 웹 API 및 웹 페이지와 통신할 수 있다. 

```bash
docker run --name favorites -d --rm -p 3000:3000 favorites-node
```
