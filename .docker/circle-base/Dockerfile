FROM ubuntu:16.04
MAINTAINER Aaron Couch aaron.couch_at_civicactions.com

# Additional Packages.
RUN apt-get update && apt-get install -y curl wget ruby php php-mbstring php-curl php-dom git 

# Install Docker from Docker Inc. repositories.
RUN curl -sSL https://get.docker.com/ | sh

# Install Docker Compose
RUN curl -L https://github.com/docker/compose/releases/download/1.11.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose

# Install Ahoy
RUN wget -q https://github.com/devinci-code/ahoy/releases/download/1.1.0/ahoy-`uname -s`-amd64 -O /usr/local/bin/ahoy && chmod +x /usr/local/bin/ahoy

# Install CircleCI CLI
RUN curl -o /usr/local/bin/circleci https://circle-downloads.s3.amazonaws.com/releases/build_agent_wrapper/circleci && chmod +x /usr/local/bin/circleci

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer global require hirak/prestissimo
ENV PATH="/root/.composer/vendor/bin:$PATH"

# Install Drush
RUN composer global require consolidation/cgr
RUN cgr drush/drush:8.0.2
RUN drush --version
