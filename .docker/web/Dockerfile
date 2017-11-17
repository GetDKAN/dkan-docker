FROM circleci/php:5.6.31-apache-node-browsers
MAINTAINER Josh Rose josh.rose_at_civicactions.com

# Install Selenium
RUN sudo npm install selenium-standalone@latest -g
RUN sudo selenium-standalone install

# Additional Packages.
RUN sudo apt-get update && sudo apt-get install -y ruby libpng-dev libmcrypt-dev mysql-client x11vnc

# Install Composer
RUN curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
RUN composer global require hirak/prestissimo
ENV PATH="/home/circleci/.composer/vendor/bin:$PATH" 

# Additional MySQL Configuration
RUN sudo chmod 777 /etc/mysql/my.cnf
RUN sudo echo -e "[client]\nloose-local-infile" >> /etc/mysql/my.cnf
RUN sudo chmod 644 /etc/mysql/my.cnf

# Additional PHP Configuration
RUN sudo chmod 777 /usr/local/etc/php/conf.d
RUN sudo echo "memory_limit = 256M" > /usr/local/etc/php/conf.d/memory.ini
RUN sudo echo "always_populate_raw_post_data = -1" > /usr/local/etc/php/conf.d/deprecated.ini
RUN sudo chmod 644 /usr/local/etc/php/conf.d

