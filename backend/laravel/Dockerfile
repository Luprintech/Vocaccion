FROM php:8.2-cli

# Instalar paquetes del sistema
RUN apt-get update && apt-get install -y \
    git zip unzip curl libzip-dev libpng-dev libpq-dev libonig-dev libxml2-dev

# Extensiones PHP necesarias por Laravel
RUN docker-php-ext-install pdo pdo_pgsql mbstring zip xml gd

# Instalar Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# Copiar proyecto
COPY . .

# Instalar dependencias Laravel
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Cachear configuración
RUN php artisan config:cache || true
RUN php artisan route:cache || true

# Render usará el puerto asignado en $PORT
EXPOSE ${PORT}

CMD php artisan migrate --force && \
    php artisan serve --host=0.0.0.0 --port=$PORT
