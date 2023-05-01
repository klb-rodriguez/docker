FROM debian:bullseye-slim

RUN cp /dev/null /etc/apt/sources.list \
    && echo "deb http://deb.debian.org/debian/ bullseye main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://deb.debian.org/debian/ bullseye main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb http://deb.debian.org/debian/ bullseye-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://deb.debian.org/debian/ bullseye-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb http://deb.debian.org/debian-security/ bullseye-security main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://deb.debian.org/debian-security/ bullseye-security main contrib non-free" >> /etc/apt/sources.list

# Instalacion de repositorio de php
RUN apt update && apt -y install apt-transport-https lsb-release ca-certificates curl wget vim aptitude \
    && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
    && sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

RUN apt update && apt install -y php7.2 php-pear php7.2-cli libapache2-mod-php7.2 php7.2-xml php7.2-dev

ENV APACHE_LOG_DIR /var/log/apache2

# logs should go to stdout / stderr
RUN ln -sfT /dev/stderr "$APACHE_LOG_DIR/error.log"; \
    ln -sfT /dev/stdout "$APACHE_LOG_DIR/access.log"; \
    ln -sfT /dev/stdout "$APACHE_LOG_DIR/other_vhosts_access.log"; \
    chown -R --no-dereference www-data:www-data "$APACHE_LOG_DIR"

# Instalando Paquetes faltantes
RUN apt update \
    && apt install -y libpq-dev zlib1g-dev libicu-dev g++ libicu67 freetds-bin freetds-common wget libxml2-dev gnupg curl \
    libpng-dev xvfb fontconfig fontconfig-config fonts-dejavu-core libfontconfig1 libjpeg62-turbo libxrender1 xfonts-75dpi \
    php7.2-pgsql php7.2-pdo php7.2-pdo-pgsql php7.2-intl php7.2-zip php7.2-soap php7.2-gd php7.2-curl wkhtmltopdf memcached libmemcached-dev \
    php7.2-igbinary php7.2-mbstring php7.2-mcrypt php7.2-redis php7.2-xdebug  php7.2-apcu php7.2-mysqlnd php7.2-pdo-sqlite php7.2-sqlite3 \
    php7.2-memcached php7.2-memcache unixodbc unixodbc-dev

# Instalar php sybase para contectarse a sql server
RUN apt update && apt -y --force-yes --no-install-recommends install freetds-dev php7.2-pdo-dblib
RUN pecl install sqlsrv-5.3.0 \
    && pecl pdo_sqlsrv-5.3.0 \
    && extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/30-pdo_sqlsrv.ini \
    && extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/20-sqlsrv.ini

COPY php.ini /usr/local/etc/php/

#Instalando Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN composer self-update --1

ADD 000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite
RUN a2enmod headers

WORKDIR /var/www/html

EXPOSE 80

CMD apachectl -D FOREGROUND