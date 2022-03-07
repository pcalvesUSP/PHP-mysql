FROM composer as build-composer

FROM php:7.4.28-fpm-alpine3.15
ARG uid=1000
ARG user=desenv
WORKDIR /var/www/
RUN apk update && apk add --no-cache\
    curl \
    zip \
    unzip \
    libpng-dev \
    oniguruma-dev \
    libxml2-dev
RUN addgroup -S wwww-data && addgroup ${user}
RUN adduser -S -G www-data -u ${uid} -h /home/${user} ${user} root
RUN mkdir -p /home/${user}/.composer && \
    chown -R ${user}:${user} /home/${user}
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd
COPY --from=build-composer /usr/bin/composer /usr/bin/composer
COPY comp*.json /var/www
RUN php /usr/bin/composer install
USER ${user}
