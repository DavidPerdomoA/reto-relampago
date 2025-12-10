FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    libpq-dev \
    zip \
    unzip \
    && docker-php-ext-install pdo pdo_pgsql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /app

# Enable mod_rewrite for Apache
RUN a2enmod rewrite

# Copy existing application code
COPY . /app

# Adjust permissions for Apache
RUN chown -R www-data:www-data /app

# Expose port 8000 and start php artisan serve
CMD sh -c "php artisan migrate --force && php artisan db:seed --force && php artisan serve --host=0.0.0.0 --port=8000"
