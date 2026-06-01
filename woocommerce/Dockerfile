FROM wordpress:latest

# 1. Install system tools required by Composer to extract packages
RUN apt-get update && apt-get install -y git unzip && rm -rf /var/lib/apt/lists/*

# 2. Leverage the official Composer image to bring in the binary cleanly
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# 3. Change context to the WordPress core layout directory
WORKDIR /usr/src/wordpress

# 4. Initialize Composer, pull WooCommerce via WPackagist, and install WP-CLI bundle
# We explicitly permit the composer/installers plugin to place the files in wp-content/plugins/
RUN composer init --no-interaction \
    && composer config allow-plugins.composer/installers true \
    && composer config repositories.wpackagist composer https://wpackagist.org \
    && composer require wpackagist-plugin/woocommerce:* wp-cli/wp-cli-bundle --no-interaction --prefer-dist \
    && ln -s /usr/src/wordpress/vendor/bin/wp /usr/local/bin/wp \
    && chown -R www-data:www-data /usr/src/wordpress