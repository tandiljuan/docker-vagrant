FROM ubuntu:12.04

RUN apt-get update
RUN apt-get -y dist-upgrade

RUN apt-get -y install php5 php5-fpm php5-gd php5-mysql \
    php5-mcrypt php5-xcache php5-xmlrpc

RUN sed -i '/daemonize /c \
daemonize = no' /etc/php5/fpm/php-fpm.conf

RUN sed -i '/^listen /c \
listen = 0.0.0.0:9000' /etc/php5/fpm/pool.d/www.conf

RUN sed -i 's/^listen.allowed_clients/;listen.allowed_clients/' /etc/php5/fpm/pool.d/www.conf


# RUN mkdir -p /srv/http && \
#     echo "<?php phpinfo(); ?>" > /srv/http/index.php && \
#     chown -R www-data:www-data /srv/http

# DATA VOLUMES
RUN mkdir -p /opt/webapp
VOLUME ["/opt/webapp"]

# PORTS
# Port 9000 is how Nginx will communicate with PHP-FPM.
EXPOSE 9000

# RUN find / -name "php-fpm" 

# Run PHP-FPM on container start.
ENTRYPOINT ["php5-fpm"]

# inspiration
# https://registry.hub.docker.com/u/dylanlindgren/docker-phpfpm/dockerfile/
# https://registry.hub.docker.com/u/jprjr/ubuntu-php-fpm/dockerfile/