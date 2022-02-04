FROM node:lts-alpine

LABEL maintainer="Lorello <lorello@devops.it>"

ARG GOMPLATE_VERSION="3.6.0"
ENV USER_HOME="/home/cyclotron"

USER root

RUN  addgroup -g 1009 cyclotron && \
     adduser -u 1009 -D -g '' -h ${USER_HOME} -G cyclotron cyclotron

RUN apk add --update --no-cache curl make ca-certificates

ADD . ${USER_HOME}/cyclotron-svc/

RUN mkdir -p ${USER_HOME}/cyclotron-svc/log

RUN curl -L -o /usr/local/bin/gomplate https://github.com/hairyhenderson/gomplate/releases/download/v${GOMPLATE_VERSION}/gomplate_linux-amd64-slim && \
    chmod +x /usr/local/bin/gomplate

WORKDIR ${USER_HOME}/cyclotron-svc

COPY --chown=cyclotron:cyclotron docker-entrypoint.sh /docker-entrypoint.sh
COPY --chown=cyclotron:cyclotron docker-image/config.js ./config/config.tpl

RUN chmod +x /docker-entrypoint.sh && \
    chown -R cyclotron:cyclotron ${USER_HOME}/cyclotron-svc && \
    npm install

USER cyclotron

EXPOSE 8077

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["node", "app.js"]

