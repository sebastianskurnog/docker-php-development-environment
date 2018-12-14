FROM php:7.3-apache

# Copy application files to contaier
COPY . /var/www/html/

# Copy PHP config file to container
COPY .docker/php/php.ini /usr/local/etc/php/

RUN apt-get update

# Install libzip
RUN apt-get install -y libzip-dev

# Install unzip
RUN apt-get install -y unzip

# Install PDO
RUN docker-php-ext-install pdo_mysql

# Install zip by docker-php-ext tool
RUN docker-php-ext-install zip

# Enable mod rewrite
RUN a2enmod rewrite

# Install APCu
RUN pecl install apcu
RUN echo "extension=apcu.so" > /usr/local/etc/php/conf.d/apcu.ini

# Install git
RUN apt-get install -y git

# Install composer and make command global
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set property permissions to files
RUN chown -R www-data:www-data /var/www/html
