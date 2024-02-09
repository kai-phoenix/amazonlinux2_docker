FROM amazonlinux:2

# timezone environment
ENV TZ=Asia/Tokyo \
    LANG=ja_JP.UTF-8 \
    LANGUAGE=ja_JP:ja \
    LC_ALL=ja_JP.UTF-8

RUN yum update -y \ 
    && yum install -y \ 
    zip \ 
    unzip \
    glibc-langpack-ja \ httpd \
    vim

RUN amazon-linux-extras install -y epel
RUN yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN yum clean metadata \
    && yum install -y \
    php81 php81-php php81-php-mbstring php81-php-pdo php81-php-xml \
    php81-php-fpm php81-php-mysqlnd php81-php-gd

RUN alternatives --install /usr/bin/php php /usr/bin/php81 1

# composer のインストール
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'edb40769019ccf227279e3bdd1f5b2e9950eb000c3233ee85148944e555d97be3ea4f40c3c2fe73b22f875385f6a5155') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1

# ファイルのコピー
COPY ./docker/php/php.ini /etc/opt/remi/php81/php.ini
COPY ./docker/apache/vhost.conf /etc/httpd/conf.d/vhost.conf
COPY ./docker/apache/rewrite.conf /etc/httpd/conf.modules.d/rewrite.conf

CMD [ "apachectl", "-D", "FOREGROUND" ]

WORKDIR /work/backend