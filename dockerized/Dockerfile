# Dockerfile for One-Time Secret

FROM debian:stretch

MAINTAINER eli fish <eli.fish@appsflyer.com>

# Install dependencies
RUN DEBIAN_FRONTEND=noninteractive \
  apt-get update && \
  apt-get install -y \
    ruby \
    ruby-dev \
    ruby-bundler \
    redis-server \
    curl \
    build-essential \
  && rm -rf /var/lib/apt/lists/*

# OTS pre-installation
RUN set -ex && \
  # Add ots user
  useradd -U -s /bin/false ots && \
  \
  # Create directories
  mkdir -p /etc/onetime /var/log/onetime /var/run/onetime /var/lib/onetime /var/lib/onetime/redis && \
  chown ots /etc/onetime /var/log/onetime /var/run/onetime /var/lib/onetime /var/lib/onetime/redis

USER ots

# Download and install latest OTS
RUN set -ex && \
  cd /var/lib/onetime && \
  git clone git@github.com:elifish4/ots.git && \
  bundle install --frozen --deployment --without=dev --gemfile /var/lib/onetime/Gemfile && \
  cp -R etc/* /etc/onetime/

  COPY --chown=ots:ots config.example /etc/onetime/config
  COPY --chown=ots:ots redis.conf.example /etc/onetime/redis.conf
  COPY entrypoint.sh /usr/bin/

VOLUME /etc/onetime /var/lib/onetime/redis

EXPOSE 7143/tcp

ENTRYPOINT /usr/bin/entrypoint.sh
