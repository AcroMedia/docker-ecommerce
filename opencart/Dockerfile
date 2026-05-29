FROM php:8.4-apache

# Install platform-required extensions
RUN apt-get update && apt-get install -y \
    libpng-dev libjpeg-dev libfreetype6-dev libzip-dev unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd mysqli zip \
    && rm -rf /var/lib/apt/lists/*

# Download and stage OpenCart webroot
ADD https://github.com/opencart/opencart/releases/download/4.0.2.3/opencart-4.0.2.3.zip /tmp/opencart.zip
RUN unzip /tmp/opencart.zip -d /tmp/opencart_dist \
    && mv /tmp/opencart_dist/upload/* /var/www/html/ \
    && mv /var/www/html/config-dist.php /var/www/html/config.php \
    && mv /var/www/html/admin/config-dist.php /var/www/html/admin/config.php \
    && chown -R www-data:www-data /var/www/html/ \
    && rm -rf /tmp/opencart.zip /tmp/opencart_dist

RUN a2enmod rewrite