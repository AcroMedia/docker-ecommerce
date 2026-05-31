FROM node:lts

WORKDIR /app

# Clone the official Medusa v2 default starter repository directly.
# This completely avoids interactive terminal prompts and TTY spinner hangs.
RUN git clone --depth=1 https://github.com/medusajs/medusa-starter-default.git medusa-store

WORKDIR /app/medusa-store

# Standard non-interactive dependency compilation
RUN npm install

EXPOSE 9000