FROM mysql:8.2
RUN mkdir /var/log/mysql
RUN chown mysql:mysql /var/log/mysql
RUN microdnf install yum \ 
    && yum install -y glibc-langpack-ja glibc-locale-source git

ENV TZ=Asia/Tokyo \
    LANG=ja\_JP.UTF-8

COPY ./docker/mysql/my.cnf /etc/my.cnf