FROM eduzz/php:7.4-cli-build

WORKDIR /src

COPY composer.json composer.lock /src/

RUN set -x \
    && composer.phar self-update

RUN set -x \
    && composer.phar install --no-ansi --no-dev --no-interaction --optimize-autoloader --no-progress --no-scripts

COPY . /src

RUN set -x \
    && composer.phar install --no-ansi --no-dev --no-interaction --optimize-autoloader --no-progress

RUN set -x \
     && chmod 755 /src/vendor -R \
     && chown -R www-data:www-data /src/vendor -R \
     && mv .env.prod .env

FROM eduzz/php:7.4-fpm

RUN apt-get update -y \
    && apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

WORKDIR /src

COPY --from=0 /src /src
COPY docker/prod/oauth-private.key /src/storage/
COPY docker/prod/oauth-public.key /src/storage/

RUN chmod 777 /src/storage -Rf \
    && chown www-data:www-data /src/storage/oauth-private.key \
    && chown www-data:www-data /src/storage/oauth-public.key \
    && chmod 600 /src/storage/oauth-private.key \
    && chmod 600 /src/storage/oauth-public.key
