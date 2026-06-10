FROM ruby:4.0

# Node and Yarn are required by the Rails asset pipeline
RUN apt-get update && apt-get install -y nodejs npm && npm install --global yarn && rm -rf /var/lib/apt/lists/*

WORKDIR /app
RUN git clone --depth=1 https://github.com/spree/spree-starter.git .

# Strip the strict Ruby version lock to gracefully handle minor image patch updates
RUN sed -i '/^ruby /d' Gemfile

# Forcefully re-route Rails to use a local standalone SQLite file
RUN cat <<EOF > config/database.yml
development:
  adapter: sqlite3
  database: db/development.sqlite3
  pool: 5
  timeout: 5000
EOF

RUN bundle config set --local deployment 'false' && bundle install

ENV RAILS_ENV=development
ENV RAILS_SERVE_STATIC_FILES=true
EXPOSE 3000

# Auto-migrate, seed demo data, and start server instantly
CMD ["sh", "-c", "bin/rails db:prepare && bin/rails db:seed && bin/rails server -b 0.0.0.0"]