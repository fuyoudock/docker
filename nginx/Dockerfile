FROM nginx:alpine

LABEL maintainer="Tylor ZHANG <tylor.zhang@outlook.com>"

###########################################################################
# Source:
###########################################################################
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories && \
    apk update && \
    apk upgrade && \
    apk add --no-cache bash

###########################################################################
# Timezone:
###########################################################################
RUN apk add --no-cache tzdata && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

###########################################################################
# Conf:
###########################################################################
COPY ./default.conf /etc/nginx/conf.d/default.conf

###########################################################################
# Clean up:
###########################################################################
RUN apk del tzdata && \
    rm -rf /var/cache/apk/* && \
    rm -rf /root/.cache && \
    rm -rf /tmp/*

EXPOSE 80

WORKDIR /var/www

CMD ["nginx", "-g", "daemon off;"]