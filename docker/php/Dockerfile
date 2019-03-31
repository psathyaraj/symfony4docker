FROM php:7.2-fpm

RUN docker-php-ext-install pdo_mysql

RUN apt-get update \
    && apt-get install -y git zip unzip

RUN pecl install apcu-5.1.8
RUN docker-php-ext-enable apcu


RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --filename=composer
RUN php -r "unlink('composer-setup.php');"
RUN mv composer /usr/local/bin/composer

WORKDIR /usr/src/app

ADD apps/symfony-app /usr/src/app

RUN PATH=$PATH:/usr/src/app/vendor/bin:bin

RUN usermod -u 1000 www-data
RUN chown -R www-data:www-data /usr/src/app

RUN mkdir -p /var/www/.composer
RUN chown -R www-data:www-data /var/www/.composer

RUN apt-get install -y zlib1g-dev
RUN docker-php-ext-install zip

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*