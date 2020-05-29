FROM node:14 as builder

## Build arguments and container enviroment variables ##

ARG API_URI
ENV API_URI ${API_URI}
ARG SALEORVERSION
ENV SALEORVERSION ${SALEORVERSION}

## Create dir for application ##

RUN mkdir -p /app/saleor-storefront

## Download selected Saleor Version & install requirements & build Saleor-Storefront ##

RUN cd /tmp \
  && git clone https://github.com/mirumee/saleor-storefront.git --branch=${SALEORVERSION} \
  && cd /tmp/saleor-storefront/ \
  && npm i \
  && API_URI=${API_URI} npm run build

## Web server to host built files ##

FROM nginx:stable

## Set directory where command is run from ##

WORKDIR /app

## copy built files from builder ##

COPY --from=builder /tmp/saleor-storefront/nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /tmp/saleor-storefront/dist/ /app/
COPY --from=builder /tmp/saleor-storefront/package*.json /app/
