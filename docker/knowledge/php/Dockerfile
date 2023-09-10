FROM php:8.2.1-fpm

# Copy composer.lock and composer.json
#COPY ./src/api/knowledge/composer.lock ./src/api/knowledge/composer.json /var/www/

#&& docker-php-ext-install intl opcache mysqli \

# Set working directory
WORKDIR /var/www

RUN apt update \
    && apt install -y zlib1g-dev g++ libicu-dev zip libzip-dev zip libpq-dev \ 
    && docker-php-ext-install intl opcache pdo pdo_mysql mysqli\
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip \
    # Add user for application
    && groupadd -g 1000 www \
    && useradd -u 1000 -ms /bin/bash -g www www    

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]