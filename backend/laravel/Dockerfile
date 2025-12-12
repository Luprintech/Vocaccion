FROM php:8.2-cli

RUN apt-get update && apt-get install -y \
    git zip unzip libpq-dev libonig-dev libxml2-dev curl

RUN docker-php-ext-install pdo pdo_pgsql mbstring xml

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

COPY . .

RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Render usará el puerto que pone en $PORT (no pongas 10000)
EXPOSE ${PORT}

CMD php artisan migrate --force && \
    php artisan config:cache && \
    php artisan route:cache && \
    php artisan serve --host=0.0.0.0 --port=$PORT
