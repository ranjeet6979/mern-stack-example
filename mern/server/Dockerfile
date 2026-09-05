FROM node:26

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 5050

#CMD ["npm","start"]
CMD ["sh", "-c", "node seed.js && npm start"]
