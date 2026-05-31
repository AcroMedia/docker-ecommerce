FROM php:8.4-apache

# Define variable for straightforward version bumps
ARG OPENCART_VERSION=4.1.0.3

# Install platform-required extensions
RUN apt-get update && apt-get install -y \
    libpng-dev libjpeg-dev libfreetype6-dev libzip-dev unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd mysqli zip \
    && rm -rf /var/lib/apt/lists/*

# Download OpenCart via the parameterized version layout
ADD https://github.com/opencart/opencart/releases/download/${OPENCART_VERSION}/opencart-${OPENCART_VERSION}.zip /tmp/opencart.zip

# Unzip and dynamically locate the 'upload' folder to handle varying archive layouts gracefully
RUN unzip /tmp/opencart.zip -d /tmp/opencart_dist \
    && TARGET_UPLOAD=$(find /tmp/opencart_dist -type d -name "upload" | head -n 1) \
    && mv "$TARGET_UPLOAD"/* /var/www/html/ \
    && mv /var/www/html/config-dist.php /var/www/html/config.php \
    && mv /var/www/html/admin/config-dist.php /var/www/html/admin/config.php \
    && chown -R www-data:www-data /var/www/html/ \
    && rm -rf /tmp/opencart.zip /tmp/opencart_dist

RUN a2enmod rewrite