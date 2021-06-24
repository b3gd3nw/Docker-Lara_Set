FROM php:7-fpm

ARG uid
RUN usermod -u ${uid} www-data

RUN apt-get update

# Development packages
RUN apt-get install -y \
    sudo \
    libonig-dev \
    libicu-dev \
    libbz2-dev \
    libpng-dev \
    libjpeg-dev \
    libmcrypt-dev \
    libreadline-dev \
    libfreetype6-dev \
    libtidy-dev \
    libzip-dev \
    npm\
    wget

# Start with base php config, then add extensions
RUN docker-php-ext-configure intl
RUN docker-php-ext-install \
    pdo_mysql

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Node Js
RUN apt-get -y install nodejs

# Add PHP ini file
ADD ./php/php.ini /usr/local/etc/php/php.ini

# Supervisor
RUN apt-get update && apt-get install -y openssh-server apache2 supervisor
RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/run/sshd /var/log/supervisor

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 22 80
CMD ["/usr/bin/supervisord"]

