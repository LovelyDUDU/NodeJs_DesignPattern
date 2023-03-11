# 83. React SPA를 컨테이너로 옮기기

```Dockerfile
FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

frontend의 Dockefile도 backend와 유사하게 진행하면 되는데 다만 컨테이너를 실행할때 `-it` (interactive mode) 옵션을 추가해야한다. 

이유는 리액트 프로젝트의 특성 때문인 것 같다. (https://mherman.org/blog/dockerizing-a-react-app/)

`-it` starts the container in interactive mode. Why is this necessary? As of version 3.4.1, `react-scripts` exits after start-up (unless CI mode is specified) which will cause the container to exit. Thus the need for interactive mode.

```bash
docker build -t goals-react .

# 에러 발생하는 경우
docker run --name goals-frontend --rm -d -p 3000:3000 goals-react 

# 에러 발생 안하는 경우
docker run --name goals-frontend --rm -d -p 3000:3000 -it goals-react 

```
