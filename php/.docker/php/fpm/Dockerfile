#FROM php:8-fpm
FROM php:7.4-fpm

ARG uid=1000
ARG user=webdev

RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip \
    libmemcached-dev

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install \
    pdo \
    pdo_mysql \
    mysqli \
    mbstring \
    exif \
    pcntl \
    bcmath \
    gd \
    opcache \
    zip

RUN pecl install \
    xdebug \
    redis \
    memcached

RUN docker-php-ext-enable \
    xdebug \
    redis \
    memcached

RUN echo "xdebug.mode=debug" >> $PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.start_with_request=1" >> $PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.discover_client_host=1" >> $PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini

RUN sed -i 's/;date.timezone =/date.timezone = America\/Sao_Paulo/' $PHP_INI_DIR/php.ini-development

RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

WORKDIR /var/www

USER $user
