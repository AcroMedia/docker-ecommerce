FROM node:lts

WORKDIR /app
# Clone the official Medusa v2 default starter repository directly.
RUN git clone --depth=1 https://github.com/medusajs/medusa-starter-default.git medusa-store

WORKDIR /app/medusa-store

# Activate Corepack and use Yarn to resolve the nested dependency graph instantly
RUN corepack enable && yarn install

EXPOSE 9000