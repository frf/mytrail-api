FROM eduzz/php:7.4-fpm

RUN set -x \
    && apt-get update -y \
    && apt-get install -y libpng-dev zlib1g-dev \
    && apt-get install -y libcurl4-openssl-dev curl libssl-dev libzip-dev zlib1g-dev zip unzip openssh-client git

RUN docker-php-ext-install gd \
    && pecl install xdebug-2.9.8 \
    && docker-php-ext-enable xdebug \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

WORKDIR /src

COPY docker/dev/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY docker/dev/fpm.conf /usr/local/etc/php-fpm.d/www.conf

RUN sed -i "s|zend_extension.*|zend_extension=$(find / -name xdebug.so)|" /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
