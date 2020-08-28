FROM php:7.0-fpm-stretch

# system packages
RUN apt-get update && apt-get install -y mc libgmp-dev libxml2-dev curl libicu-dev libpng-dev libjpeg-dev zlib1g-dev wget git unzip \
    && pecl install xdebug-2.7.1 \
    && docker-php-ext-enable xdebug

# php extensions
RUN docker-php-ext-install -j$(nproc) pdo_mysql zip mbstring opcache exif calendar gettext simplexml \
    gmp mysqli intl gd


# composer
RUN wget https://getcomposer.org/installer -O - -q \
    | php -- --install-dir=/bin --filename=composer --quiet
RUN composer --version

COPY ./docker/app/config.ini $PHP_INI_DIR/conf.d/config.ini
COPY ./docker/app/www.conf /usr/local/etc/php-fpm.d/www.conf

# timezone
ARG TIMEZONE="UTC"
RUN echo ${TIMEZONE} > /etc/timezone \
    && rm /etc/localtime \
    && ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata \
    && apt-get clean \
    && echo "date.timezone = ${TIMEZONE}" >> /usr/local/etc/php/conf.d/tz.ini

WORKDIR /var/www/netcat
CMD ["php-fpm"]