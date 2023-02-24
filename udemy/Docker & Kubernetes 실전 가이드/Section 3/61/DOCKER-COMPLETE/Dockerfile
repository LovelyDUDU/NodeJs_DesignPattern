FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# docker 내부에서 PORT=80 이라는 환경변수를 설정
ENV PORT=80

EXPOSE $PORT

CMD ["npm", "start"]