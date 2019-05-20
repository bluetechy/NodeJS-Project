FROM node:latest

RUN apt-get update && apt-get install -y vim

RUN mkdir -p var/www/app

COPY ./src/ /var/www/app/
COPY ./package.json /var/www/package.json

RUN cd /var/www && npm install --only=production --loglevel=error
RUN npm install -g nodemon --loglevel=error

EXPOSE 8080

WORKDIR /var/www
