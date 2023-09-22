FROM node:16

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

ENTRYPOINT ["node", "app.js"]
#docker build -t node-application:0.0.1 .
#docker run -d node-application:0.0.1 .
