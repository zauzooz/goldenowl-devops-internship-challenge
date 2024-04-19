# REPORT

## DOCKERNIZES NODEJS APP

Tại thư mục `src/`, tạo một tệp `Dockerfile` với nội dung như sau:

```
FROM node:10-alpine

RUN mkdir -p /home/honeypot/intern_app

RUN mkdir -p /home/honeypot/intern_app/node_modules

WORKDIR /home/honeypot/intern_app

COPY package*.json ./

RUN adduser -D honeypot

RUN chown -R honeypot:honeypot /home/honeypot/intern_app

RUN chmod -R 755 /home/honeypot/intern_app/node_modules

USER honeypot

RUN npm install

COPY --chown=honeypot:honeypot . .

EXPOSE 3000

CMD [ "node", "index.js" ]
```

Build image by using `sudo docker build -t honeypot/demo .`

Running docker image by using `sudo docker run --name demo -p 3000:3000 -d honeypot/demo`

## REFERENCE
