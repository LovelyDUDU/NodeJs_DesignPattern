FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

# VOLUEM ["/app/node_modules"]

CMD ["node", "server.js"]