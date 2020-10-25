FROM php:latest


RUN apt-get update && apt-get install -y \
        curl \
        git

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
WORKDIR /var/www
ENTRYPOINT ["php-fpm"]
