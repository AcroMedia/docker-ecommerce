FROM node:lts

WORKDIR /app
# Clone the official Medusa v2 default starter repository directly.
RUN git clone --depth=1 https://github.com/medusajs/medusa-starter-default.git medusa-store

WORKDIR /app/medusa-store

# Set required fallback parameters to pass Medusa's build-time validation layers
ENV JWT_SECRET=supersecret
ENV COOKIE_SECRET=supersecret
ENV NODE_ENV=development

# Enable Corepack, download dependencies flatly, and pre-compile the server bundle
RUN corepack enable && yarn install && yarn build

EXPOSE 9000