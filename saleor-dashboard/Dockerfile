FROM python:3.8-slim as builder

## Build arguments and container enviroment variables ##

ENV PYTHONUNBUFFERED 1
ENV PROCESSES 4
ARG API_URI
ENV API_URI ${API_URI}
ARG APP_MOUNT_URI
ENV APP_MOUNT_URI ${APP_MOUNT_URI}
ARG SALEORVERSION
ENV SALEORVERSION ${SALEORVERSION}
ARG STATIC_URL
ENV STATIC_URL ${STATIC_URL}

## Install required packages ##

RUN apt-get update \
  && apt-get install -y \
    git \
    curl \
    npm \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

## Install NPM 14 ##

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get update \
  && apt-get install -y nodejs

## Create dir for application ##

RUN mkdir -p /app/saleor-dashboard/

## Download selected Saleor Version & install requirements then build Saleor-dashboard##

RUN cd /tmp \
  && git clone https://github.com/mirumee/saleor-dashboard.git --branch=${SALEORVERSION} \
  && cd /tmp/saleor-dashboard/ \
  && npm i \
  && npm run build \
  && cp -r /tmp/saleor-dashboard/build/ /app/saleor-dashboard \
  && cp /tmp/saleor-dashboard/package*.json /app/saleor-dashboard

## Set directory where command is run from ##
 
WORKDIR /app/saleor-dashboard

FROM nginx:stable

WORKDIR /app
COPY --from=builder /tmp/saleor-dashboard/nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /tmp/saleor-dashboard/build/ /app/
COPY --from=builder /tmp/saleor-dashboard/package*.json /app/

