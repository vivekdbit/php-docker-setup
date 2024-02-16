FROM public.ecr.aws/docker/library/php:7.4-fpm

# Set working directory
WORKDIR /var/www/html

# Install dependencies
RUN apt-get update && apt-get install -y git unzip libssl-dev libcurl4-openssl-dev libpng-dev libzip-dev zlib1g-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*


# Install extensions
RUN docker-php-ext-install pdo_mysql zip exif pcntl mysqli gd

#Install librdkafka version 2.1.1
#RUN cd /tmp && mkdir librdkafka && cd librdkafka \
#    && git clone --branch v2.1.1 https://github.com/edenhill/librdkafka.git . \
#    && ./configure \
#    && make \
#    && make install \
#    && pecl install rdkafka && docker-php-ext-enable rdkafka \
#    && rm -rf /tmp/librdkafka

# RUN pecl install xdebug
# RUN docker-php-ext-enable xdebug

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www/html

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer install --no-dev --optimize-autoloader

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
