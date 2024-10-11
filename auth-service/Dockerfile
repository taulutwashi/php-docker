# Use PHP 8.0+ base image with Apache
FROM php:8.0-apache

# Set working directory inside the container
WORKDIR /var/www/html

# Install system dependencies and PHP extensions for Lumen
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libzip-dev \
    unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo pdo_mysql mbstring gd zip

# Enable Apache mod_rewrite for clean URLs
RUN a2enmod rewrite

# Copy the current directory contents into the container
COPY .. .

# Install Composer (dependency manager for PHP)
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install project dependencies using Composer
RUN composer install

# Expose port 80
EXPOSE 80

# Start Apache when the container launches
CMD ["apache2-foreground"]
