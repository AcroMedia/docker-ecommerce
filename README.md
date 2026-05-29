# Multi-Platform E-Commerce Evaluation Guide

This guide provides simple, minimal steps for users to evaluate various e-commerce platforms using simple Docker commands. 

---

### 1. WooCommerce
```bash
# 1. Start Database
docker run -d --name woo-db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=wordpress mysql:8

# 2. Start WooCommerce
docker run -d -p 8080:80 --name woocommerce --link woo-db:mysql -e WORDPRESS_DB_PASSWORD=secret my-woocommerce
```

Access URL: http://localhost:8080


### 2. OpenCart
```bash
# 1. Start Database
docker run -d --name opencart-db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=opencart mysql:8

# 2. Start OpenCart
docker run -d -p 8080:80 --name opencart --link opencart-db:mysql my-opencart
```

Access URL: http://localhost:8080


### 3. Shopware
Uses a fully standalone image built by the community that includes an embedded database out of the box.
```bash
docker run -d -p 8080:80 --name shopware dockware/shopware:latest
```

Access URL: http://localhost:8080

Admin Panel: http://localhost:8080/admin (User: admin / Pass: shopware)


### 4. SpreeCommerce
A completely self-contained Ruby on Rails engine setup utilizing an internal SQLite profile for instant preview.
```bash
# Start Spree
docker run -d -p 3000:3000 --name spree my-spree
```

Access URL: http://localhost:3000

Admin Panel: http://localhost:3000/admin (User: spree@example.com / Pass: spree123)


### 5. Saleor
Launches the official Saleor Core engine. Note that this spins up the backend GraphQL API playground directly.
```bash
# 1. Start Database
docker run -d --name saleor-db -e POSTGRES_PASSWORD=secret -e POSTGRES_DB=saleor postgres:15

# 2. Run Migrations & Start Saleor Core API
docker run -it -p 8000:8000 --name saleor --link saleor-db:db -e DATABASE_URL=postgres://postgres:secret@db:5432/saleor ghcr.io/saleor/saleor:latest sh -c "python manage.py migrate && python manage.py populatedb && python manage.py runserver 0.0.0.0:8000"
```
Access URL: http://localhost:8000/graphql/


### 6. Medusa
An open-source headless engine paired alongside a standard datastore container.
```bash
# 1. Start Database
docker run -d --name medusa-db -e POSTGRES_PASSWORD=secret -e POSTGRES_DB=medusa postgres:15

# 2. Start Medusa Engine
docker run -d -p 9000:9000 --name medusa --link medusa-db:db -e DATABASE_URL=postgres://postgres:secret@db:5432/medusa my-medusa sh -c "npx medusa db:migrate && npm run start"
```
Access URL: http://localhost:9000/health

### 7. Drupal Commerce
Evaluates Drupal Commerce using a pre-packaged optimization layer image.
```bash
# 1. Start Database
docker run -d --name drupal-db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=drupal mariadb:latest

# 2. Start Drupal Commerce Container
docker run -d -p 8080:80 --name drupal-commerce --link drupal-db:mysql insready/drupal-commerce:11
```
Access URL: http://localhost:8080