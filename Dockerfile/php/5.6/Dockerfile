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

RUN apt update && apt install -y php5.6 php-pear php5.6-cli libapache2-mod-php5.6 php5.6-xml php5.6-dev

ENV APACHE_LOG_DIR /var/log/apache2

# logs should go to stdout / stderr
RUN ln -sfT /dev/stderr "$APACHE_LOG_DIR/error.log"; \
    ln -sfT /dev/stdout "$APACHE_LOG_DIR/access.log"; \
    ln -sfT /dev/stdout "$APACHE_LOG_DIR/other_vhosts_access.log"; \
    chown -R --no-dereference www-data:www-data "$APACHE_LOG_DIR"

# Instalando Paquetes faltantes
RUN apt-get update \
    && apt-get install -y libpq-dev zlib1g-dev libicu-dev g++ libicu67 freetds-bin freetds-common wget libxml2-dev gnupg curl \
    libpng-dev xvfb fontconfig fontconfig-config fonts-dejavu-core libfontconfig1 libjpeg62-turbo libxrender1 xfonts-75dpi \
    php5.6-pgsql php5.6-pdo php5.6-pdo-pgsql php5.6-intl php5.6-zip php5.6-soap php5.6-gd php5.6-curl \
    php5.6-igbinary php5.6-mbstring php5.6-mcrypt php5.6-redis php5.6-xdebug  php5.6-apcu php5.6-mysqlnd php5.6-pdo-sqlite php5.6-sqlite3

# Instalar php sybase para contectarse a sql server
RUN apt-get -y --force-yes --no-install-recommends install freetds-dev php5.6-mssql php5.6-pdo-dblib php5.6-pdo-dblib

# Instalacion de WKHTMLTOPDF
# RUN wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.buster_amd64.deb \
RUN wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_arm64.deb \
    # && dpkg --install wkhtmltox_0.12.5-1.buster_amd64.deb
    && dpkg --install wkhtmltox_0.12.6-1.buster_arm64.deb

COPY php.ini /etc/php/5.6/apache2/

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