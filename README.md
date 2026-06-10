# Multi-Platform E-Commerce Evaluation Guide

This guide provides simple, minimal steps for users to evaluate various e-commerce platforms using simple Docker commands. 

---

## Image Strategy & Selection Audit

This repository maintains custom evaluation images for platforms lacking lightweight, single-container community alternatives, while leveraging robust existing configurations where available:

| # | Platform | Target Docker Image Tag / Source | Image Type | Architectural Rationale / Why |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **WooCommerce** | `acromedia/ecommerce:woocommerce` | **Custom Built** | No active official standalone image exists. Built via Composer and WPackagist to inject the plugin safely into an official WordPress base image at build time. |
| **2** | **OpenCart** | `acromedia/ecommerce:opencart` | **Custom Built** | Official Bitnami images are deprecated. Built with dynamic directory discovery to handle changing `.zip` structures across official releases resiliently. |
| **3** | **Shopware** | `dockware/shopware:latest` | **Community** | Leverages the highly trusted community `dockware` image, which includes an embedded database footprint out of the box for zero-config run loops. |
| **4** | **SpreeCommerce** | `acromedia/ecommerce:spree` | **Custom Built** | The official cloud-ready image requires active external Postgres and Redis clusters to build. This image uses Ruby 4.x and a standalone SQLite profile. |
| **5** | **Saleor** | `ghcr.io/saleor/saleor:latest` | **Official** | Well-maintained official core API engine. This guide provisions a direct datastore link and bootstraps schema creation at execution runtime. |
| **6** | **Medusa** | `acromedia/ecommerce:medusa` | **Custom Built** | The official startup workflows force multi-container Docker Compose. This image pre-scaffolds the Node workspace cleanly for a lightweight startup loop. |
| **7** | **Drupal Commerce** | `insready/drupal-commerce:11` | **Custom Built** | Leverages your production-grade baseline layout container optimizing modern dependency caching layers and Drupal 11 core. |

---

---

### 1. WooCommerce
```bash
# 1. Start Database
docker run -d --name woo-db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=wordpress mysql:8

# 2. Start WooCommerce
docker run -d -p 8080:80 --name woocommerce --link woo-db:mysql -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=secret acromedia/ecommerce:woocommerce
```

Access URL: http://localhost:8080
Note: Complete the basic WordPress screen wizard; WooCommerce will be waiting for activation inside the 'Plugins' menu.


### 2. OpenCart
```bash
# 1. Start Database
docker run -d --name opencart-db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=opencart mysql:8

# 2. Start OpenCart
docker run -d -p 8080:80 --name opencart --link opencart-db:mysql acromedia/ecommerce:opencart
```

Access URL: http://localhost:8080
Note: Follow the graphical web installer steps using: Database Host: `mysql`, User: `root`, Password: `secret`.


### 3. Shopware
Uses a fully standalone image built by the community that includes an embedded database out of the box.
```bash
docker run -d -p 8080:80 --name shopware dockware/shopware:latest
```

Access URL: http://localhost:8080

Admin Panel: http://localhost:8080/admin (User: `admin` / Pass: `shopware`)


### 4. SpreeCommerce
A completely self-contained Ruby on Rails engine setup utilizing an internal SQLite profile for instant preview.
```bash
# Start Spree
docker run -d -p 3000:3000 --name spree acromedia/ecommerce:spree
```

Access URL: http://localhost:3000

Admin Panel: http://localhost:3000/admin (User: `spree@example.com` / Pass: `spree123`)


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
# 1. Start Backing Database
docker run -d --name medusa-db -e POSTGRES_PASSWORD=secret -e POSTGRES_DB=medusa postgres:15

# 2. Execute Internal Migrations & Run Engine via Yarn
docker run -d -p 9000:9000 --name medusa --link medusa-db:db -e DATABASE_URL=postgres://postgres:secret@db:5432/medusa acromedia/ecommerce:medusa sh -c "npx medusa db:migrate && yarn start"
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