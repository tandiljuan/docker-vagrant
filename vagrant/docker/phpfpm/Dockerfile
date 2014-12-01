FROM ubuntu:12.04

RUN apt-get update
RUN apt-get -y dist-upgrade

RUN apt-get -y install php5 php5-fpm php5-mysql \
    php5-xcache php5-xmlrpc

# Wordpress Requirements
RUN apt-get -y install php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap \
    php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-sqlite php5-tidy php5-xmlrpc php5-xsl

RUN sed -i '/daemonize /c \
daemonize = no' /etc/php5/fpm/php-fpm.conf

RUN sed -i '/^listen /c \
listen = 0.0.0.0:9000' /etc/php5/fpm/pool.d/www.conf

RUN sed -i 's/^listen.allowed_clients/;listen.allowed_clients/' /etc/php5/fpm/pool.d/www.conf

# new ones
# RUN sed -i -e "s/;catch_workers_output\s*=\s*yes/catch_workers_output = yes/g" /etc/php5/fpm/pool.d/www.conf
# RUN sed -i "s/;php_admin_flag\[log_errors\] = .*/php_admin_flag[log_errors] = true/" /etc/php5/fpm/pool.d/www.conf
# RUN sed -i "s/;php_flag\[display_errors\] = .*/php_flag[display_errors] = on/" /etc/php5/fpm/pool.d/www.conf
RUN sed -i "s/;php_admin_value\[memory_limit\] = .*/php_admin_value[memory_limit] = 128M/" /etc/php5/fpm/pool.d/www.conf

# DATA VOLUMES
RUN mkdir -p /opt/webapp
VOLUME ["/opt/webapp"]

# PORTS
# Port 9000 is how Nginx will communicate with PHP-FPM.
EXPOSE 9000

# Run PHP-FPM on container start.
ENTRYPOINT ["php5-fpm"]


# inspiration
# https://registry.hub.docker.com/u/dylanlindgren/docker-phpfpm/dockerfile/
# https://registry.hub.docker.com/u/jprjr/ubuntu-php-fpm/dockerfile/