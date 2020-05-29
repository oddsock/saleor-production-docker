FROM python:3.8-slim

## Build arguments and container enviroment variables ##

ARG ALLOWED_HOSTS
ENV ALLOWED_HOSTS ${ALLOWED_HOSTS}
ARG SALEORVERSION
ENV SALEORVERSION ${SALEORVERSION}
ARG SECRET_KEY
ENV SECRET_KEY ${SECRET_KEY}
ARG STATIC_URL
ENV STATIC_URL ${STATIC_URL:-/static/}
ARG DATABASE_URL
ENV DATABASE_URL ${DATABASE_URL}
ENV PYTHONUNBUFFERED 1
ENV PROCESSES 4

## Install required packages ##

RUN apt-get update \
  && apt-get install -y \
    libxml2 \
    libssl1.1 \
    libcairo2 \
    libpango-1.0-0 \
    libpangocairo-1.0-0 \
    libgdk-pixbuf2.0-0 \
    shared-mime-info \
    mime-support \
    git \
    npm \
    build-essential \
    libffi-dev \
    curl \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

## Install NPM 14 ##

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get update \
  && apt-get install -y nodejs

## Create dir for application ##

RUN mkdir /app

## Download selected Saleor Version & install requirements & install Saleor ##

RUN cd /app \
  && git clone https://github.com/mirumee/saleor.git --branch=${SALEORVERSION} \
  && cd saleor \
  && pip install -r requirements.txt \
  && cd /app/saleor/ \
  && npm install \
  && npm run build-schema \
  && npm run build-emails 

## Set directory where command is run from ##

WORKDIR /app/saleor

