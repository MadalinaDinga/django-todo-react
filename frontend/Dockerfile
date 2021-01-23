FROM node:10.19.0

RUN mkdir /app
WORKDIR /app
COPY package.json .
RUN npm install

COPY . .

CMD ["npm", "start"]