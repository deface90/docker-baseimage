FROM alpine:3.15

LABEL maintainer="deface <deface06@yandex.ru>"

ENV \
    TERM=xterm-color              \
    TIME_ZONE=Asia/Yekaterinburg  \
    APP_USER=app                  \
    APP_UID=1001                  \
    DOCKER_GID=999         

COPY app.sh /usr/bin/app.sh
COPY init.sh /init.sh

RUN \
    chmod +x /usr/bin/app.sh /init.sh && \
    apk add --no-cache --update su-exec tzdata curl ca-certificates dumb-init && \
    ln -s /sbin/su-exec /usr/local/bin/gosu && \
    mkdir -p /home/$APP_USER && \
    adduser -s /bin/sh -D -u $APP_UID $APP_USER && chown -R $APP_USER:$APP_USER /home/$APP_USER && \
    delgroup ping && addgroup -g 998 ping && \
    addgroup -g ${DOCKER_GID} docker && addgroup ${APP_USER} docker && \
    mkdir -p /srv && chown -R $APP_USER:$APP_USER /srv && \
    cp /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime && \
    echo "${TIME_ZONE}" > /etc/timezone && date && \
    ln -s /usr/bin/dumb-init /sbin/dinit && \
    rm -rf /var/cache/apk/*

ENTRYPOINT ["/init.sh"]
