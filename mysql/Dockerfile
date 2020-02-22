ARG MYSQL_VERSION=latest

FROM mysql:${MYSQL_VERSION}

LABEL maintainer="Tylor ZHANG <tylor.zhang@outlook.com>"

COPY my.cnf /etc/mysql/conf.d/my.cnf

RUN chmod 0444 /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306
