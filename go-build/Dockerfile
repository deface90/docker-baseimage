FROM golang:1.12-alpine
LABEL maintainer="deface <deface06@yandex.com>"

ENV \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64 \
    # GOVERALLS=0.0.2 \
    GOLANGCI=1.17.1

RUN \
    apk add --no-cache --update tzdata git bash curl && \
    cp /usr/share/zoneinfo/Asia/Yekaterinburg /etc/localtime && \
    rm -rf /var/cache/apk/*

RUN \
    go version && \
    go get -u -v github.com/GoASTScanner/gas && \
    go get -u -v github.com/golang/dep/cmd/dep && \
    go get -u -v github.com/stretchr/testify

RUN \
    go get -u -v github.com/golangci/golangci-lint/cmd/golangci-lint && \
    cd /go/src/github.com/golangci/golangci-lint && \
    git checkout v${GOLANGCI} && \
    cd /go/src/github.com/golangci/golangci-lint/cmd/golangci-lint && \
    go install -ldflags "-X 'main.version=$(git describe --tags)' -X 'main.commit=$(git rev-parse --short HEAD)' -X 'main.date=$(date)'" && \
    golangci-lint --version

# RUN \
#     go get -u -v github.com/mattn/goveralls && \
#     cd /go/src/github.com/mattn/goveralls && \
#     git checkout v${GOVERALLS} && \
#     go install github.com/mattn/goveralls

ADD coverage.sh /script/coverage.sh
ADD git-rev.sh /script/git-rev.sh

RUN chmod +x /script/coverage.sh /script/git-rev.sh
