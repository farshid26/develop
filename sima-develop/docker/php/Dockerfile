## Use official PHP image as a parent image
#FROM php:8.3-fpm
#
## Install dependencies
#RUN docker-php-ext-install pdo pdo_mysql
#
## Set the working directory in the container
#WORKDIR /var/www/html
#
#RUN chown -R www-data:www-data /var/www/html
FROM php:8.3-fpm
WORKDIR /var/www/html
#ARG uid
#ARG gid

## Create laravel user and group
#RUN addgroup --gid ${gid} --system laravel
#RUN adduser --gid ${gid} --system --shell /bin/sh -u ${uid} laravel
#
## Update PHP-FPM configuration to use laravel user/group
#RUN sed -i "s/user = www-data/user = laravel/g" /usr/local/etc/php-fpm.d/www.conf
#RUN sed -i "s/group = www-data/group = laravel/g" /usr/local/etc/php-fpm.d/www.conf

# Set up PHP-FPM process management
RUN sed -i "s/pm.start_servers =.*/pm.start_servers = 20/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i "s/pm.min_spare_servers =.*/pm.min_spare_servers = 20/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i "s/pm.max_spare_servers =.*/pm.max_spare_servers = 60/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i "s/pm.max_children =.*/pm.max_children = 100/g" /usr/local/etc/php-fpm.d/www.conf
RUN echo 'pm.status_path = /status' | tee -a /usr/local/etc/php-fpm.d/zz-docker.conf

# Install OPcache and other PHP extensions
RUN docker-php-ext-install opcache
RUN echo 'zend_extension=opcache.so' | tee -a /usr/local/etc/php/php.ini
RUN echo 'opcache.enable=1' | tee -a /usr/local/etc/php/php.ini
RUN echo 'opcache.enable_cli=1' | tee -a /usr/local/etc/php/php.ini
RUN echo 'opcache.jit_buffer_size=500000000' | tee -a /usr/local/etc/php/php.ini
RUN echo 'opcache.jit=1235' | tee -a /usr/local/etc/php/php.ini

RUN echo '[opcache]' | tee -a /usr/local/etc/php/conf.d/opcache.ini
RUN echo 'opcache.enable=1' | tee -a /usr/local/etc/php/conf.d/opcache.ini
RUN echo 'opcache.revalidate_freq=0' | tee -a /usr/local/etc/php/conf.d/opcache.ini
RUN echo 'opcache.validate_timestamps=1' | tee -a /usr/local/etc/php/conf.d/opcache.ini
RUN echo 'opcache.max_accelerated_files=10000' | tee -a /usr/local/etc/php/conf.d/opcache.ini
RUN echo 'opcache.memory_consumption=192' | tee -a /usr/local/etc/php/conf.d/opcache.ini
RUN echo 'opcache.max_wasted_percentage=10' | tee -a /usr/local/etc/php/conf.d/opcache.ini
RUN echo 'opcache.interned_strings_buffer=16' | tee -a /usr/local/etc/php/conf.d/opcache.ini
RUN echo 'opcache.fast_shutdown=1' | tee -a /usr/local/etc/php/conf.d/opcache.ini

# Install necessary dependencies
RUN apt-get update && apt-get install -y libpng-dev libzip-dev zip git
RUN docker-php-ext-install pdo pdo_mysql gd zip

# Copy the application code
COPY . /var/www/html

# Set the working directory
WORKDIR /var/www/html

# Install Composer
#RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer
#RUN alias composer='php /usr/bin/composer'
#RUN composer install

# Set correct ownership and permissions
RUN chown -R www-data:www-data /var/www/html
RUN chmod -R 755 /var/www

#USER laravel
