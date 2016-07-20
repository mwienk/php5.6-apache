FROM php:5.6-apache
MAINTAINER Mark Wienk <mark@wienkit.nl>

ENV DEBIAN_FRONTEND noninteractive
ENV NODE_VERSION 4.4.5

RUN requirements="libpng12-dev libjpeg-dev libjpeg62-turbo libmcrypt4 libmcrypt-dev libcurl3-dev libxml2-dev libxslt-dev libicu-dev libicu52 libxslt-dev openssh-client nodejs-legacy npm mysql-server git libbz2-dev libfreetype6 libfontconfig" \
    && apt-get update && apt-get install -y $requirements && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure gd --with-jpeg-dir=/usr/lib \
    && docker-php-ext-install gd \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install soap \
    && docker-php-ext-install xsl \
    && docker-php-ext-install intl \
    && docker-php-ext-install zip \
    && docker-php-ext-install bz2 \
    && echo "always_populate_raw_post_data=-1" > /usr/local/etc/php/php.ini \
    && echo "memory_limit=-1" > /usr/local/etc/php/php.ini

RUN curl -sSL https://getcomposer.org/composer.phar -o /usr/bin/composer \
    && chmod +x /usr/bin/composer \
    && apt-get update && apt-get install -y zlib1g-dev git && rm -rf /var/lib/apt/lists/* \
    && composer selfupdate \
    && composer global require symfony/console:2.7.9 codegyre/robo:1.0.0-beta1 drush/drush:dev-master

ENV PATH /root/.composer/vendor/bin:$PATH

RUN npm install -g gulp

RUN sed -i "s/DocumentRoot \/var\/www\/html/DocumentRoot \/var\/www\/docroot/" /etc/apache2/apache2.conf \
    && rm -rf /var/www && mkdir -p /var/www

VOLUME ["/var/www"]

WORKDIR /var/www

RUN usermod -u 1000 www-data
RUN chown -R www-data:www-data /var/www
