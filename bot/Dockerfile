FROM debian:latest

RUN apt update
RUN apt install curl build-essential git python3 python3-pip -y

SHELL ["/bin/bash", "--login", "-c"]
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

WORKDIR /app/ZeppelinBot
ARG GIT_REPO=https://github.com/Dragory/ZeppelinBot.git
ARG GIT_BRANCH=master
RUN git clone $GIT_REPO . && git switch $GIT_BRANCH

RUN nvm install

WORKDIR /app/ZeppelinBot/backend
RUN npm i

RUN npm run build

ARG DB_DATABASE
ARG DB_PASSWORD
ARG DB_PORT
ARG DB_USER
ARG DB_HOST
ARG API_URL
ARG DASHBOARD_URL
ARG KEY
ARG STAFF
ARG CLIENT_SECRET
ARG CLIENT_ID
ARG BOT_TOKEN
ARG RAILWAY_SERVICE_API_URL


# Wait for API to be available

RUN echo "Waiting for API Url $API_URL to be available.."
RUN timeout 300 bash -c 'while [[ "$(curl -s -o /dev/null -w ''%{http_code}'' $API_URL)" != "200" ]]; do sleep 5; done' || false

ENTRYPOINT ["/bin/bash", "--login", "-c", "npm run start-bot-prod"]
