FROM ubuntu:16.04

RUN apt-get update
RUN apt-get install software-properties-common -y
RUN apt-get update

# The basics
RUN apt-get install -y \
	supervisor \
	curl \
	wget \
	unzip \
  locales \
	git \
	pv \
	apt-transport-https \
	vim \
	patch \
  zip 

RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update
# PHP 5
RUN apt-get install -y \
  php \
  php-mbstring \
  php-mcrypt \
  mysql-client\
  php-curl \
  php-mysql \
  php-xml \
  php-gd \
  php-pear \
  php-memcache \
  php-dev \
  php-xdebug \
  php-cli \
  php-mcrypt

RUN php -v 
RUN pecl install xhprof-0.9.4

# Install Nodejs and Ruby
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash
RUN apt-get install -y nodejs

# Install Ruby
RUN apt-get install -y \
  rlwrap \
  ruby \
  ruby-dev

# Cleanup
RUN DEBIAN_FRONTEND=noninteractive apt-get clean && \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Bundler
RUN gem install bundler

# Grunt, Bower
RUN npm install -g grunt-cli bower

# Install theme dependencies
ADD package.json /tmp/package.json
RUN cd /tmp && npm install
RUN mkdir -p /opt/app && cp -a /tmp/node_modules /opt/app/

# Install Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/bin/composer
RUN chmod +x /usr/bin/composer

# Install Drush
RUN COMPOSER_HOME=/opt/drush COMPOSER_BIN_DIR=/usr/local/bin COMPOSER_VENDOR_DIR=/opt/drush/8 composer require drush/drush:^8
RUN cd /opt/drush/8/drush/drush && composer install 

# Allow composer superuser and set environment to use composer executables path
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV PATH "$PATH:/root/.composer/vendor/bin"

# Install PHPCS and Drupal Coding Standards
RUN composer global require squizlabs/php_codesniffer
RUN composer global require drupal/coder

# Set Drupal as default CodeSniffer Standard
RUN phpcs --config-set installed_paths /root/.composer/vendor/drupal/coder/coder_sniffer/
RUN phpcs --config-set default_standard Drupal

# Add local settings
COPY php-cli.ini /etc/php/7.0/cli/conf.d/z_php.ini
# Add xdebug settins. These are moved when `ahoy docker xdebug start` is run.
COPY xdebug-linux.ini /etc/php5/xdebug-linux.ini
COPY xdebug-macos.ini /etc/php5/xdebug-macos.ini

# Add git completion for the cli
RUN curl -o ~/.git-completion.bash https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash
RUN curl -o ~/.git-prompt.sh https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh
COPY bash.rc /root/.bashrc

WORKDIR /var/www

# Add Composer bin directory to PATH
ENV PATH /root/.composer/vendor/bin:$PATH

# Home directory for bundle installs
ENV BUNDLE_PATH .bundler

# SSH settigns
COPY .ssh /root/.ssh

# Add registry_rebuild
RUN drush dl registry_rebuild-7.x

RUN wget -q https://s3-us-west-1.amazonaws.com/nucivic-binaries/ahoy/1.1.0/ahoy-linux-amd64 -O /usr/local/bin/ahoy && \
    chmod +x /usr/local/bin/ahoy

# Startup script
COPY ./startup.sh /opt/startup.sh
RUN chmod +x /opt/startup.sh

# Starter script
ENTRYPOINT ["/opt/startup.sh"]

# By default, launch supervisord to keep the container running.
CMD /usr/bin/supervisord -n
