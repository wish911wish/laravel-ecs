FROM node:14.2-alpine as node
FROM nginx:1.18-alpine as NGINX_RUNTIME

SHELL ["/bin/ash", "-oeux", "pipefail", "-c"]

ENV TZ=UTC

RUN apk update && \
  apk add --update --no-cache --virtual=.build-dependencies g++

# node command
COPY --from=node /usr/local/bin /usr/local/bin
# npm command
COPY --from=node /usr/local/lib /usr/local/lib
# yarn command
COPY --from=node /opt /opt
# nginx config file
COPY ./infrastructure/docker/nginx/*.conf /etc/nginx/conf.d/

RUN mkdir -p /work/backend/public

WORKDIR /work/backend

FROM NGINX_RUNTIME as NGINX_APPLICATION_BUNDLED

COPY ./backend/public /work/backend/public
