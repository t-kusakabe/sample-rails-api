##########################
## Builder Container
##########################
FROM ruby:3.0.1-alpine3.13 as builder

WORKDIR /app

RUN apk update && \
    apk add --no-cache \
      build-base \
      libxml2-dev \
      libxslt-dev \
      linux-headers \
      libpq \
      mysql-dev && \
    gem install bundler

COPY Gemfile Gemfile.lock ./

RUN bundle install

##########################
## Application Container
##########################
FROM ruby:3.0.1-alpine3.13

WORKDIR /app

# NOTE: ruby2.7の警告を消す
ENV RUBYOPT="-W:no-deprecated -W:no-experimental"

RUN apk update && \
    apk add --no-cache \
      build-base \
      curl-dev \
      libxml2-dev \
      libxslt-dev \
      linux-headers \
      libpq \
      mysql-client \
      mysql-dev \
      less \
      nodejs \
      tzdata && \
    gem install bundler

COPY --from=builder /usr/local/bundle /usr/local/bundle
COPY . .

CMD ["bin/rails", "server", "-b", "0.0.0.0"]
