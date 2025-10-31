FROM node:18-alpine

RUN apk update && \
    apk add --no-cache curl tini bash && \
    rm -rf /var/cache/apk/*

WORKDIR /usr/local/app

RUN npm install -g nodemon

COPY package*.json ./

RUN npm install && \
    npm cache clean --force && \
    mv /usr/local/app/node_modules /node_modules

RUN apk update && \
    apk upgrade && \
    rm -rf /var/cache/apk/*

COPY . .

ENV PORT=80
EXPOSE 80

CMD ["node", "server.js"]
