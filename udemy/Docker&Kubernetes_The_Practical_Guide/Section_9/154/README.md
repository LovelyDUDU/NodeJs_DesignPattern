# 154. 멀티 스테이지 이미지 구축

동일한 서버의 api를 호출할 때는 localhost를 사용하지 않는다.

```bash
# -f: docker file의 이름을 지정할 때 사용
docker build -f frontend/Dockerfile.prod -t harry/goals-react ./frontend
```

