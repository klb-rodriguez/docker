FROM php:8.1-zts-bullseye

> RUN cp /dev/null /etc/apt/sources.list \
>     && echo "deb http://deb.debian.org/debian/ bullseye main contrib non-free" >> /etc/apt/sources.list \
>     && echo "deb-src http://deb.debian.org/debian/ bullseye main contrib non-free" >> /etc/apt/sources.list \
>     && echo "deb http://deb.debian.org/debian/ bullseye-updates main contrib non-free" >> /etc/apt/sources.list \
>     && echo "deb-src http://deb.debian.org/debian/ bullseye-updates main contrib non-free" >> /etc/apt/sources.list \
>     && echo "deb http://deb.debian.org/debian-security/ bullseye-security main contrib non-free" >> /etc/apt/sources.list \
>     && echo "deb-src http://deb.debian.org/debian-security/ bullseye-security main contrib non-free" >> /etc/apt/sources.list
>

# Instalacion de repositorio de php
# RUN apt update && apt -y install apt-transport-https lsb-release ca-certificates curl wget vim aptitude \
#     && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
#     && sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

#Instalando Xdebug
# RUN pecl install xdebug \
#     && docker-php-ext-enable xdebug \
#     && echo "xdebug.overload_var_dump=On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#     && docker-php-ext-install opcache \
#     && docker-php-ext-enable opcache \
#     && echo "opcache.file_cache = /var/www/html/.opcache" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

#Instalando apc
# RUN printf "\n" | pecl install apcu \
#     && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini

# Instalando Paquetes faltantes
# RUN apt-get update \
#     && apt-get install -y nano zip libzip-dev libpq-dev zlib1g-dev libicu-dev g++ libxml2-dev memcached libmemcached-dev \
#     wkhtmltopdf freetds-bin freetds-common wget lsb-release gnupg libxslt1-dev libpng-dev \
#     && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
#     && docker-php-ext-install pdo pdo_pgsql pgsql \
#     && docker-php-ext-install intl xml gd \
#     && docker-php-ext-configure zip \
#     && docker-php-ext-install xsl

# Instalando mysqli
# RUN docker-php-ext-install mysqli pdo_mysql

# COPY php.ini /usr/local/etc/php/

# Instalando memcached
# RUN pecl install memcached \
#     && docker-php-ext-enable memcached

# Instalando Couchbase lib
# RUN wget https://packages.couchbase.com/clients/c/repos/deb/couchbase.key \
#     && apt-key add couchbase.key \
#     && echo "deb https://packages.couchbase.com/clients/c/repos/deb/debian11 bullseye bullseye/main" > /etc/apt/sources.list.d/couchbase.list \
#     && apt update \
#     && apt-get install -y libcouchbase3 libcouchbase-dev libcouchbase3-tools libcouchbase-dbg libcouchbase3-libev libcouchbase3-libevent \
#     && pecl install couchbase \
#     && docker-php-ext-enable couchbase

#Instalando Composer
# RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
#     && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
#     && php -r "unlink('composer-setup.php');"


# ADD 000-default.conf /etc/apache2/sites-available/000-default.conf

# RUN a2enmod rewrite
# RUN a2enmod headers