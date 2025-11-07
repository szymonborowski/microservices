# Używamy oficjalnego obrazu PHP z FPM (FastCGI Process Manager)
FROM php:8.4-fpm

WORKDIR /var/www/html

RUN apt-get update && apt-get install -y \
    git \
    curl \
    zip \
    unzip \
    libpq-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install \
        pdo \
        pdo_pgsql \
        mbstring \
        exif \
        pcntl \
        bcmath \
        gd \
        zip \
    && rm -rf /var/lib/apt/lists/*

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

COPY . .

ENV COMPOSER_HOME=/composer

RUN mkdir -p $COMPOSER_HOME && chmod -R 777 $COMPOSER_HOME

RUN composer global require laravel/installer

ENV PATH="$COMPOSER_HOME/vendor/bin:$PATH"

RUN chown -R www-data:www-data /var/www/html

USER www-data

CMD ["php-fpm"]