FROM node:12

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 3045

CMD [ "npm", "start" ]