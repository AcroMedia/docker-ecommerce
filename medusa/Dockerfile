FROM node:lts

WORKDIR /app
# Pre-scaffold project seamlessly at build time to shorten evaluation startup loop.
# Pass 'medusa-store' without path prefixes to avoid naming validation errors.
RUN echo "N" | npx create-medusa-app@latest medusa-store --skip-db

WORKDIR /app/medusa-store
EXPOSE 9000