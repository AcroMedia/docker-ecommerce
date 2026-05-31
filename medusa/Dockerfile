FROM node:lts

WORKDIR /app
# Pre-scaffold project seamlessly at build time to shorten evaluation startup loop.
# Full Debian includes git, make, and compiler chains natively.
RUN echo "N" | npx create-medusa-app@latest ./medusa-store --skip-db

WORKDIR /app/medusa-store
EXPOSE 9000