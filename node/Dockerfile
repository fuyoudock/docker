ARG NODE_VERSION=lts

FROM node:${NODE_VERSION}-alpine

LABEL maintainer="Tylor ZHANG <tylor.zhang@outlook.com>"

###########################################################################
# Source:
###########################################################################
RUN sed -i 's/dl-cdn.alpinelinux.org/mirror.tuna.tsinghua.edu.cn/' /etc/apk/repositories && \
    apk update && \
    apk upgrade && \
    apk add --no-cache bash python make g++

###########################################################################
# Timezone:
###########################################################################
RUN apk add --no-cache tzdata && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

###########################################################################
# Registry:
###########################################################################
RUN npm config set registry https://registry.npm.taobao.org && \
    yarn config set registry https://registry.npm.taobao.org

###########################################################################
# Clean up:
###########################################################################
RUN apk del tzdata && \
    rm -rf /var/cache/apk/* && \
    rm -rf /root/.cache && \
    rm -rf /tmp/*

WORKDIR /var/www