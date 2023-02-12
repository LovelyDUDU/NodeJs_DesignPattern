# 39.5. 퀴즈

Q1. 이 명령의 결과는 무엇인가? 
```bash
docker build -t myimage .
docker run --name mycontainer myimage
docker stop mycontainer
```

A1. 이미지가 생성되고, 컨테이너가 시작된 다음, 중지된다. 이미지와 컨테이너 모두 개발자가 지정한 이름이 있다.

</br>

Q2. 다음 명령이 실행되었다고 가정한다.
```bash 
docker build -t myimage:latest
docker run --name mycontainer --rm myimage
docker stop mycontainer
```
다음 명령중 어떤 것이 실패할까?

A2. 
```bash
# --rm 옵션떄문에 컨테이너가 중지되녀 자동으로 제거되기 때문
docker rm mycontainer
```

</br>

Q3. 이미지 태그의 배경 사상은 무엇인가?

A3. 이미지에는 이름이 있을 수 있으며, 그 이름의 여러 '버전' 이 같은 이름에 첨부될 수 있다.
(ex node:14)

</br>

Q4. 커스텀 이미지 태그와 컨테이너 이름을 할당해야 하는가?

A4. X, Docker는 이름과 id를 자동으로 할당한다.