FROM ubuntu:18.04

ARG DEBIAN_FRONTEND=noninteractive

RUN \
  apt-get update && \
  apt-get install -o Dpkg::Options::=--force-confdef -y apache2  apache2-dev

RUN \
    apt-get update && \
    apt-get install -y software-properties-common && \
    apt-get install -y imagemagick graphicsmagick && \
    apt-get install -y libapache2-mod-php7.2 php7.2-bcmath php7.2-bz2 php7.2-cli php7.2-common php7.2-curl php7.2-dba php7.2-gd php7.2-imap php7.2-intl php7.2-ldap php7.2-mbstring php7.2-mysql php7.2-odbc php7.2-xml php7.2-xmlrpc php7.2-xsl php7.2-zip php7.2-xdebug && \
    apt-get install -y php-imagick

# Disable loading of xdebug.so
RUN rm -f /etc/php/7.2/cli/conf.d/20-xdebug.ini && \
    rm -f /etc/php/7.2/apache2/conf.d/20-xdebug.ini

RUN a2enmod rewrite

RUN a2enmod ssl && \
    a2ensite default-ssl

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
COPY default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

EXPOSE 80 443

ENV PHP_INI_SCAN_DIR="/etc/php/7.2/apache2/conf.d:/var/www/src/docker/etc/php"

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
