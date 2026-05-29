FROM wordpress:latest

# Install unzip to extract the plugin
RUN apt-get update && apt-get install -y unzip && rm -rf /var/lib/apt/lists/*

# Download and bake WooCommerce into the core WordPress plugin directory
ADD https://downloads.wordpress.org/plugin/woocommerce.latest-stable.zip /tmp/woocommerce.zip
RUN unzip /tmp/woocommerce.zip -d /usr/src/wordpress/wp-content/plugins/ && rm /tmp/woocommerce.zip