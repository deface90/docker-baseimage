FROM golang:1.18-alpine
LABEL maintainer="deface <deface06@yandex.com>"

ENV \
    TIME_ZONE=Asia/Yekaterinburg \
    CGO_ENABLED=0                \
    GOVERALLS=0.0.11             \
    GOLANGCI=1.45.2              

RUN \
    apk add --no-cache --update tzdata git bash curl && \
    echo "${TIME_ZONE}" > /etc/timezone && \
    cp /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime && \
    date && \
    rm -rf /var/cache/apk/*

RUN \
    go version && \
    go install github.com/matryer/moq@latest && moq -version

RUN \
    go install github.com/golangci/golangci-lint/cmd/golangci-lint@v${GOLANGCI} && \
    golangci-lint --version

RUN go install github.com/mattn/goveralls@v${GOVERALLS} && which goveralls

ADD coverage.sh /script/coverage.sh
ADD git-rev.sh /script/git-rev.sh

RUN chmod +x /script/coverage.sh /script/git-rev.sh
