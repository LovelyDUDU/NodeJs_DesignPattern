# 87. (바인드 마운트로) React 컨테이너에 대한 라이브 소스 코드 업데이트하기

기존 컨테이너 명령어
```bash
docker run --name goals-frontend --rm -p 3000:3000 -it goals-react
```

바인드 마운트를 사용하여 소스코드의 변경사항이 자동적으로 적용되게 할 예정이다.

소스코드 폴더를 컨테이너의 `/app/src` 에 바인딩한다.

```bash
docker run -v $pwd:/app/src --name goals-frontend --rm -p 3000:3000 -it goals-react
```