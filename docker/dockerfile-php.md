---
layout: default
title: Dockerfile PHP
has_children: true
---

# Dockerfile PHP

```bash
FROM php:8.1-apache

ENV APACHE_SERVER_NAME website

RUN sed -i 's!/var/www/html!/var/www/public!g' /etc/apache2/sites-available/000-default.conf

WORKDIR /var/www

RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libxml2-dev \
    libssl-dev \
    libexif-dev \
    libmagickwand-dev --no-install-recommends \
    unzip

RUN pecl install imagick

RUN docker-php-ext-configure gd \
    && docker-php-ext-enable imagick \
    && docker-php-ext-install -j$(nproc) gd \
    #&& docker-php-ext-install bcmath \
    #&& docker-php-ext-install ctype \
    #&& docker-php-ext-install exif \
    #&& docker-php-ext-install json \
    #&& docker-php-ext-install mbstring \
    #&& docker-php-ext-install openssl \
    && docker-php-ext-install xml

# Install Composer globally
#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

COPY . /var/www/

RUN composer update && \
    composer install && \
    php artisan optimize && \
    php please cache:clear && \
    php please stache:refresh
```
