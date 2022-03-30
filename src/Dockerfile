FROM php:7.4-fpm-alpine

LABEL maintainer="cyrusyun@qq.com"

RUN echo "http://mirrors.aliyun.com/alpine/v3.15/main" > /etc/apk/repositories \
    && echo "http://mirrors.aliyun.com/alpine/v3.15/community" >> /etc/apk/repositories \
    && echo "http://dl-2.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories \
    && apk update upgrade \
    && apk add --no-cache procps unzip curl bash tzdata \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone

RUN apk add --no-cache libxml2-dev libpng libpng-dev libjpeg libjpeg-turbo-dev freetype-dev libzip-dev git autoconf zlib unixodbc-dev shadow libstdc++

RUN docker-php-ext-install mysqli pdo_mysql zip sockets bcmath
RUN docker-php-ext-configure gd --with-freetype=/usr/local/include/ --with-jpeg=/usr/local/include/ \
    && docker-php-ext-install -j$(nproc) gd
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com \
    && composer self-update --clean-backups

RUN apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && pecl install pdo_sqlsrv \
    && docker-php-ext-enable pdo_sqlsrv \
    && pecl clear-cache \
    && usermod -u 1000 www-data \
    && rm -rf /usr/share/php \
    && rm -rf /tmp/* \
    && apk del .phpize-deps \
    && rm -rf /var/cache/apk/*
