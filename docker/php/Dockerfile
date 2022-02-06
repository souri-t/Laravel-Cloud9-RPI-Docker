FROM php:7.2-fpm-alpine3.7
COPY php.ini /usr/local/etc/php/

# PHP-Laravel
RUN apk --no-cache update && \
    apk --no-cache upgrade && \
    apk --no-cache add \
    curl-dev \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libxml2-dev \
    zlib-dev \
    pcre-dev \
    g++ \
    make \
    autoconf \
    openssl \
    nodejs-npm \
    bash \
    vim \
    git \
  && docker-php-ext-install \
    curl \
    dom \
    mbstring \
    pdo \
    pdo_mysql \
    simplexml \
    zip \
    opcache \
  && docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
  && docker-php-ext-install gd \
  && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
  && chmod +x /usr/local/bin/composer \
  && rm -rf /var/cache/apk/*

#Cloud9
RUN apk add --update --no-cache bash git nodejs make gcc g++ python curl wget build-base openssl-dev apache2-utils libxml2-dev sshfs tmux supervisor \
    && rm -f /var/cache/apk/* \
    && cd ~ \
    && git clone https://github.com/c9/core.git /c9 \
    && curl -s -L https://raw.githubusercontent.com/c9/install/master/link.sh | bash \
    && cd /c9 \
    && ./scripts/install-sdk.sh

VOLUME ["/var/www"]
WORKDIR /var/www

EXPOSE 8080

ENV USERNAME user
ENV PASSWORD pass

ENTRYPOINT ["sh", "-c", "/usr/bin/node /c9/server.js -l 0.0.0.0 -p 8080 -w /var/www -a $USERNAME:$PASSWORD"]
