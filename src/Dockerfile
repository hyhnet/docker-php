FROM php:7.1-fpm-alpine

MAINTAINER hyhnet <cyrusyun@qq.com>

RUN echo "http://mirrors.aliyun.com/alpine/v3.8/main" > /etc/apk/repositories \
    && echo "http://mirrors.aliyun.com/alpine/v3.8/community" >> /etc/apk/repositories \
    && apk update upgrade \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS procps unzip curl bash tzdata \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone

RUN apk add libxml2-dev libpng libpng-dev libjpeg freetype-dev

RUN docker-php-ext-install mysqli pdo_mysql zip sockets bcmath
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/local/include/ --with-png-dir=/usr/local/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN pecl install redis \
    && docker-php-ext-enable redis \
    && pecl clear-cache \
    && rm -rf /usr/share/php \
    && rm -rf /tmp/* \
    && apk del  .phpize-deps \
    && rm -rf /var/cache/apk/*