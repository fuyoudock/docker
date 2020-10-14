ARG PYTHON_VERSION=3.8

FROM python:${PYTHON_VERSION}-alpine

LABEL maintainer="Tylor ZHANG <tylor.zhang@outlook.com>"

###########################################################################
# Source:
###########################################################################
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories && \
    apk update && \
    apk upgrade && \
    apk add --no-cache bash mariadb-dev gcc libc-dev libxml2-dev libxslt-dev  libffi-dev

###########################################################################
# Timezone:
###########################################################################
RUN apk add --no-cache tzdata && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

###########################################################################
# Pylint:
###########################################################################
RUN pip install -i https://mirrors.aliyun.com/pypi/simple --no-cache-dir pylint

###########################################################################
# Clean up:
###########################################################################
RUN apk del tzdata && \
    rm -rf /var/cache/apk/* && \
    rm -rf /root/.cache && \
    rm -rf /tmp/*

WORKDIR /var/www