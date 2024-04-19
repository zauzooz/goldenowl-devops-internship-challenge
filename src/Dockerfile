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
