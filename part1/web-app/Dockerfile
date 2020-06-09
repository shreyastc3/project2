FROM node:14.2.0-slim

WORKDIR web-app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000
CMD ["node", "app.js"]
