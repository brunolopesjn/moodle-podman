FROM php:8.4-apache

# Dependências necessárias ao Moodle
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libicu-dev \
    libzip-dev \
    libxml2-dev \
    libonig-dev \
 && docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install \
    gd \
    intl \
    zip \
    mysqli \
    soap \
    mbstring \
    opcache \
 && apt-get clean

RUN docker-php-ext-install exif

RUN echo "zend.exception_ignore_args = On" \
    > /usr/local/etc/php/conf.d/99-moodle-security.ini

RUN echo "max_input_vars = 5000" \
    > /usr/local/etc/php/conf.d/99-moodle-input.ini

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Código do Moodle
WORKDIR /var/www/html
RUN git clone -b MOODLE_501_STABLE https://github.com/moodle/moodle.git .

# Composer install (OBRIGATÓRIO)
RUN composer install --no-dev --classmap-authoritative

# Copia configurações
COPY config/config.php /var/www/html/config.php
COPY config/000-default.conf /etc/apache2/sites-available/000-default.conf

# Habilita módulos necessários
RUN a2enmod rewrite \
 && chown -R www-data:www-data /var/www/html

EXPOSE 80
