# 105. ENTRYPOINT 활용


Dockerfile에서 ENTRYPOINT 에 `[ "npm" ]` 를 설정해주면 원래 
```
docker run -it -v "/Users/harry/Project/TIL/udemy/Docker&Kubernetes_The_Practical_Guide/Section_7/105/docker-complete":/app mynpm npm install
```

였던 명령어를 

```
docker run -it -v "/Users/harry/Project/TIL/udemy/Docker&Kubernetes_The_Practical_Guide/Section_7/105/docker-complete":/app mynpm install
```

로 줄여서 쓸 수 있다.

ENTRYPOINT 와 CMC 의 가장 큰 차이점은 컨테이너 시작시 실행 명령에 대한 default 지정 여부이다.

CMD: 컨테이너를 실행할 때 인자값을 주게 되면 Dockerfile에 지정된 CMD 값을 대신하여 지정한 인자값으로 변경하여 실행되게 된다.

ENTRYPOINT: 컨테이너가 수행될 때 반드시 ENTRYPOINT에서 지정한 명령이 수행되도록 지정된다.