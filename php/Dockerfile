ARG PHP_VERSION=7.2

FROM php:${PHP_VERSION}-fpm-alpine

LABEL maintainer="Tylor ZHANG <tylor.zhang@outlook.com>"

###########################################################################
# Source:
###########################################################################
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories && \
    apk update && \
    apk upgrade && \
    apk add --no-cache bash gcc g++ autoconf make git graphviz font-bitstream-type1 && \ 
    apk add --no-cache --repository http://dl-3.alpinelinux.org/alpine/edge/testing gnu-libiconv
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

###########################################################################
# Timezone:
###########################################################################
RUN apk add --no-cache tzdata && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

###########################################################################
# Nginx:
###########################################################################
COPY nginx.conf /etc/nginx/
RUN apk add --no-cache nginx && \
    echo "upstream php-upstream { server 127.0.0.1:9000; }" > /etc/nginx/conf.d/upstream.conf && \
    rm /etc/nginx/conf.d/default.conf
    # chown -R www-data:root /var/tmp/nginx

###########################################################################
# Php ini:
###########################################################################
RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

###########################################################################
# Composer:
###########################################################################
RUN apk add --no-cache composer && \
    composer self-update && \
    composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

###########################################################################
# Php mysql:
###########################################################################
RUN docker-php-ext-install pdo pdo_mysql

###########################################################################
# Php gd:
###########################################################################
RUN apk add --no-cache freetype freetype-dev libpng libpng-dev libjpeg-turbo libjpeg-turbo-dev && \
    docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install gd

###########################################################################
# Php imap:
###########################################################################
RUN apk add --no-cache imap-dev openssl-dev && \
    docker-php-ext-configure imap --with-imap --with-imap-ssl && \
    docker-php-ext-install imap

###########################################################################
# Php opcache:
###########################################################################
COPY ./opcache.ini temp/opcache.ini
RUN docker-php-ext-install opcache && \
    cp temp/opcache.ini $PHP_INI_DIR/conf.d/

###########################################################################
# Php xml/dom:
###########################################################################
RUN apk add --no-cache libxml2-dev && \
    docker-php-ext-install xml dom

###########################################################################
# Php imagick:
###########################################################################
RUN apk add --no-cache imagemagick imagemagick-libs imagemagick-dev && \
    pecl install imagick && \
    docker-php-ext-enable imagick
                
###########################################################################
# Php xdebug:
###########################################################################
ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    pecl install xdebug && \
    docker-php-ext-enable xdebug \
;fi

###########################################################################
# Php intl:
###########################################################################
RUN apk add --no-cache icu icu-libs icu-dev && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl

###########################################################################
# Php xsl:
###########################################################################
RUN apk add --no-cache libxslt-dev libgcrypt-dev && \
    docker-php-ext-install xsl

###########################################################################
# Clean up:
###########################################################################
RUN apk del tzdata && \
    rm -rf /var/cache/apk/* && \
    rm -rf /root/.cache && \
    rm -rf /tmp/*


ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash", "/opt/startup.sh"]

WORKDIR /var/www

EXPOSE 80