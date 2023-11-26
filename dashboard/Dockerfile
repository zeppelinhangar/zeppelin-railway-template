FROM nginx

ARG API_URL
ARG DOMAIN_NAME
ARG NODE_ENV
ARG API_PROXY_FQDN

RUN apt-get update
RUN apt install curl build-essential git python3 python3-pip openssl wget -y
RUN openssl req -x509 -newkey rsa:4096 -keyout /etc/ssl/private/zeppelin-self-signed-cert.key -out /etc/ssl/certs/zeppelin-self-signed-cert.pem -days 3650 -subj "/CN=${DOCKER_PROD_DOMAIN}" -nodes

SHELL ["/bin/bash", "--login", "-c"]
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash


WORKDIR /etc/nginx/sites-available

COPY . .
RUN sed -ir "s/_API_PROXY_FQDN_/${API_PROXY_FQDN}/g" zeppelin
RUN sed -ir "s/_DOCKER_PROD_DOMAIN_/${DOMAIN_NAME}/g" zeppelin
RUN rm /etc/nginx/conf.d/default.conf
RUN ln -s /etc/nginx/sites-available/zeppelin /etc/nginx/conf.d/default.conf
#RUN sed -ir "s/root \/zeppelin\/dashboard\/dist/root \/dashboard\/dist/g" /etc/nginx/conf.d/default.conf
#RUN sed -ir "s/443 ssl http2/80/g" /etc/nginx/conf.d/default.conf

WORKDIR /app/ZeppelinBot
ARG GIT_REPO=https://github.com/Dragory/ZeppelinBot.git
ARG GIT_BRANCH=master
RUN git clone $GIT_REPO . && git switch $GIT_BRANCH

RUN nvm install 16
RUN touch .env
RUN echo "API_URL=$API_URL" > .env
RUN cd backend && npm i --production=false
RUN cd dashboard && npm i --production=false && npx cross-env NODE_ENV=web webpack --config webpack.config.js

EXPOSE 80
#EXPOSE 443
