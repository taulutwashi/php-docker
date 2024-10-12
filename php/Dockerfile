FROM php:8.1-fpm-alpine as app

# php extension installer image
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin

RUN set -eux; \
    docker-php-ext-install pdo pdo_mysql
#RUN docker-php-ext-install pdo pdo_mysql

ENV COMPOSER_ALLOW_SUPERUSER=1

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY ./app/composer.* ./

RUN composer install --prefer-dist --no-dev --no-scripts --no-progress --no-interaction

COPY ./app .

RUN composer dump-autoload --optimize

## Dev version staging

FROM app as dev_app

ENV XDEBUG_MODE=off

COPY ./php/conf.d/xdebug.ini /usr/local/etc/php/config.d/xdebug.ini

RUN set -eux; \
    install-php-extensions xdebug
