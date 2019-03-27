FROM ubuntu:18.04

RUN apt-get update && apt-get update && apt-get install apache2 -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install php libapache2-mod-php -y && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    DEBIAN_FRONTEND=noninteractive apt-get install php7.1 -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install php7.1-dev php7.1-cli php7.1-common php7.1-curl php7.1-gd php7.1-json php7.1-opcache php7.1-mysql php7.1-mbstring php7.1-mcrypt php7.1-zip php7.1-xml php7.1-xdebug -y

# Disable loading of xdebug.so
RUN rm -f /etc/php/7.1/cli/conf.d/20-xdebug.ini && \
    rm -f /etc/php/7.1/apache2/conf.d/20-xdebug.ini

RUN a2enmod rewrite && \
    a2dismod php7.2 && \
    a2enmod php7.1

RUN update-alternatives --set php /usr/bin/php7.1 && \
    update-alternatives --set phar /usr/bin/phar7.1 && \
    update-alternatives --set phar.phar /usr/bin/phar.phar7.1 && \
    update-alternatives --set phpize /usr/bin/phpize7.1 && \
    update-alternatives --set php-config /usr/bin/php-config7.1 && \
    a2enmod ssl && \
    a2ensite default-ssl

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
COPY default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

EXPOSE 80 443

ENV PHP_INI_SCAN_DIR="/etc/php/7.1/apache2/conf.d:/var/www/src/docker/etc/php"

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]

