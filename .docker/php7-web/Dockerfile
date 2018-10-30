FROM ubuntu:18.04

RUN apt update
RUN apt-get update

RUN apt install apache2 -y

RUN DEBIAN_FRONTEND=noninteractive apt install php libapache2-mod-php -y
RUN apt-get install -y software-properties-common

RUN add-apt-repository ppa:ondrej/php

RUN DEBIAN_FRONTEND=noninteractive apt install php7.1 -y
RUN DEBIAN_FRONTEND=noninteractive apt install php7.1-dev php7.1-cli php7.1-common php7.1-curl php7.1-gd php7.1-json php7.1-opcache php7.1-mysql php7.1-mbstring php7.1-mcrypt php7.1-zip php7.1-xml -y

RUN a2enmod rewrite
RUN a2dismod php7.2
RUN a2enmod php7.1

RUN update-alternatives --set php /usr/bin/php7.1
RUN update-alternatives --set phar /usr/bin/phar7.1
RUN update-alternatives --set phar.phar /usr/bin/phar.phar7.1 
RUN update-alternatives --set phpize /usr/bin/phpize7.1
RUN update-alternatives --set php-config /usr/bin/php-config7.1

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
COPY default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

RUN a2ensite default-ssl
EXPOSE 80 443

# Generate SSL certificate and key
RUN openssl req -batch -nodes -newkey rsa:2048 -keyout /etc/ssl/private/server.key -out /tmp/server.csr && \
    openssl x509 -req -days 365 -in /tmp/server.csr -signkey /etc/ssl/private/server.key -out /etc/ssl/certs/server.crt;rm /tmp/server.csr
    
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

