ARG MYSQL_VERSION=latest

FROM mysql:${MYSQL_VERSION}

LABEL maintainer="Tylor ZHANG <tylor.zhang@outlook.com>"

#####################################
# Set Timezone
#####################################

ARG TZ=Asia/Shanghai
ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo $TZ > /etc/timezone && chown -R mysql:root /var/lib/mysql/

COPY my.cnf /etc/mysql/conf.d/my.cnf

RUN chmod 0444 /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306
