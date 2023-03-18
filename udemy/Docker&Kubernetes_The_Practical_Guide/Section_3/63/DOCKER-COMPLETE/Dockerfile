FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# ARG = 빌드 타임 인수
ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT

EXPOSE $PORT

CMD ["npm", "start"]