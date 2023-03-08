FROM node:14.2.0-slim

WORKDIR web-app

COPY package*.json ./

RUN pip install

COPY . .

EXPOSE 80
CMD ["node", "app.js"]
