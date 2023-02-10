FROM node

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . /app

# 여기서 docker 포트를 80 으로 했기 때문에 컨테이너를 실행할 때 포트를 지정해줘야한다.
# 예를 들면 docker run -p 3000:80 sha:~~ 이런식으로
EXPOSE 80 

CMD ["node", "server.js"]