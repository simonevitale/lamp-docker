# Apache2

FROM php:8.0-apache

RUN a2enmod rewrite

RUN apt-get update \
    && apt-get install -y git zip unzip curl sudo libicu-dev libbz2-dev libpng-dev libjpeg-dev libmcrypt-dev libreadline-dev libfreetype6-dev g++

WORKDIR /var/www/html

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions gd pdo_mysql bcmath zip intl opcache

COPY --from=composer:2.0 /usr/bin/composer /usr/local/bin/composer

# Let's Encrypt

ENV TZ=Europe/London
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# RUN rm /etc/apache2/sites-enabled/000-default.conf \
#   && rm /etc/apache2/sites-enabled/default-ssl.conf

RUN apt-get update && apt-get install software-properties-common -y

RUN add-apt-repository "deb http://archive.ubuntu.com/ubuntu $(lsb_release -sc) universe" \
  && add-apt-repository ppa:certbot/certbot -y \
  && apt-get clean -y

RUN apt-get install certbot python3-certbot-apache -y

# COPY index.html /var/www/html/index.html

COPY /docker/server/launcher.sh /usr/local/bin/launcher.sh
RUN chmod +x /usr/local/bin/launcher.sh

CMD /usr/local/bin/launcher.sh