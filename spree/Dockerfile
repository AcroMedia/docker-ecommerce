# Why the Official Image Fails the "Instant Evaluation" Test
# If you try to run docker run -p 3000:3000 ghcr.io/spree/spree, the container will instantly crash. Here is why:
# - Hardcoded Infrastructure Dependencies: The official image is designed as a stateless 12-factor app. It mandates external microservices. It will fail on startup if it does not receive a valid DATABASE_URL (pointing to a live PostgreSQL/MySQL instance) and a REDIS_URL (for background workers and caching).
# - Missing Assets & Seed Data: The official image contains the core engine code but does not pre-bake an initialized storefront with dummy products, images, or default admin accounts. Setting that up requires launching the container, running database creation tasks, and executing separate seeding commands.
# - No Embedded SQLite Support: The official build is stripped of SQLite support to optimize production image sizes, requiring you to manage multiple containers even if you just want to take a 5-minute look at the dashboard.

FROM ruby:4.0-slim

RUN apt-get update && apt-get install -y \
    build-essential git nodejs npm sqlite3 libsqlite3-dev \
    && npm install --global yarn && rm -rf /var/lib/apt/lists/*

WORKDIR /app
RUN git clone --depth=1 https://github.com/spree/spree-starter.git .
RUN bundle config set --local deployment 'false' && bundle install

ENV RAILS_ENV=development
ENV RAILS_SERVE_STATIC_FILES=true
EXPOSE 3000

# Auto-migrate, seed demo data, and start server instantly
CMD ["sh", "-c", "bin/rails db:prepare && bin/rails db:seed && bin/rails server -b 0.0.0.0"]