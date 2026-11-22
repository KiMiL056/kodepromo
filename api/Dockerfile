# syntax=docker/dockerfile:1

FROM php:8.4-cli-alpine AS deps
WORKDIR /var/www/html

RUN apk add --no-cache \
    git \
    unzip \
    libzip-dev \
    oniguruma-dev \
    icu-dev \
    mysql-client \
    $PHPIZE_DEPS \
    && docker-php-ext-install -j$(nproc) \
        pdo_mysql \
        mbstring \
        bcmath \
        intl \
        zip \
        opcache

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

COPY composer.json composer.lock ./
RUN composer install --no-interaction --prefer-dist --no-scripts --no-autoloader

COPY . .
RUN composer dump-autoload --optimize

FROM deps AS test
RUN php artisan test

FROM php:8.4-cli-alpine AS runtime
WORKDIR /var/www/html

RUN apk add --no-cache \
    libzip \
    oniguruma \
    icu-libs \
    mysql-client \
    curl

COPY --from=deps /usr/local/lib/php/extensions/ /usr/local/lib/php/extensions/
COPY --from=deps /usr/local/etc/php/conf.d/ /usr/local/etc/php/conf.d/
COPY --from=deps /usr/bin/composer /usr/bin/composer
COPY --from=deps /var/www/html/vendor /var/www/html/vendor
COPY --from=deps /var/www/html /var/www/html

RUN rm -f .env \
    && mkdir -p storage/framework/cache storage/framework/sessions storage/framework/views bootstrap/cache \
    && chmod -R 775 storage bootstrap/cache

EXPOSE 8000

CMD ["sh", "-c", "php artisan config:clear && php artisan migrate --force && exec php artisan serve --host=0.0.0.0 --port=8000"]
