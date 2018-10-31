FROM ubuntu:18.04

RUN apt update && apt-get update && apt install apache2 -y && \
    DEBIAN_FRONTEND=noninteractive apt install php libapache2-mod-php -y && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    DEBIAN_FRONTEND=noninteractive apt install php7.1 -y && \
    DEBIAN_FRONTEND=noninteractive apt install php7.1-dev php7.1-cli php7.1-common php7.1-curl php7.1-gd php7.1-json php7.1-opcache php7.1-mysql php7.1-mbstring php7.1-mcrypt php7.1-zip php7.1-xml -y && \

    a2enmod rewrite && \
    a2dismod php7.2 && \
    a2enmod php7.1 && \

    update-alternatives --set php /usr/bin/php7.1 && \
    update-alternatives --set phar /usr/bin/phar7.1 && \
    update-alternatives --set phar.phar /usr/bin/phar.phar7.1 && \
    update-alternatives --set phpize /usr/bin/phpize7.1 && \
    update-alternatives --set php-config /usr/bin/php-config7.1 && \

    a2enmod ssl && \
    a2ensite default-ssl

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
COPY default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

EXPOSE 80 443

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
