FROM ubuntu:14.04

RUN apt-get update
RUN apt-get -y dist-upgrade

# Install dependencies
RUN apt-get install -y wget curl git zip unzip

# Install HHVM
RUN wget -O - http://dl.hhvm.com/conf/hhvm.gpg.key | apt-key add -
RUN echo deb http://dl.hhvm.com/ubuntu trusty main | tee /etc/apt/sources.list.d/hhvm.list
RUN apt-get update
RUN apt-get install -y hhvm

# HHVM command
ENV HHVM_COMMAND /usr/bin/hhvm -v ResourceLimit.SocketDefaultTimeout=600 -v Http.SlowQueryThreshold=600000

# Composer path
ENV COMPOSER_PATH /opt/composer

# Donwload and Install Composer
RUN mkdir -p $COMPOSER_PATH
WORKDIR $COMPOSER_PATH
RUN wget -q https://getcomposer.org/installer
RUN $HHVM_COMMAND installer

# Install Laravel
WORKDIR /opt/laravel
CMD if [ "$(ls -A laravel)" ]; then \
        # Install/update and existent Laravel app
        cd laravel; \
        $HHVM_COMMAND $COMPOSER_PATH/composer.phar install --prefer-dist --no-dev --no-scripts; \
    else \
        # Install a new Laravel app
        $HHVM_COMMAND $COMPOSER_PATH/composer.phar create-project laravel/laravel --prefer-dist --no-dev --no-scripts; \
    fi \
    # Make the storage folder writable by anyone
    && chmod -R 777 /opt/laravel/laravel/app/storage
