FROM node:20-alpine
RUN apk add --no-cache python3 make g++ git

WORKDIR /app
# Pre-scaffold project seamlessly at build time to shorten evaluation startup loop
RUN npx create-medusa-app@latest --with-nextjs-storefront=false --skip-db-init ./medusa-store

WORKDIR /app/medusa-store
EXPOSE 9000