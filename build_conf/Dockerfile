# Deployable PHP image
FROM php:8.1-apache-bullseye

# install the PHP extensions we need
RUN set -eux; \
  \
  apk update; \
  apk add gnupg mysql-client zlib-dev libzip-dev autoconf  build-base; \
  apk add \
  libjpeg-turbo-dev \
  libpng-dev \
  libwebp-dev \
  freetype-dev \
  optipng \
  advancecomp \
  ; \
  \
  docker-php-ext-configure gd \
  --with-freetype \
  --with-jpeg=/usr \
  --with-webp \
  ; \
  \
  docker-php-ext-install -j "$(nproc)" \
  bcmath \
  gd \
  exif \
  opcache \
  pdo_mysql \
  zip \
  ; \
  \

WORKDIR /opt/drupal

COPY . /opt/drupal

RUN set -eux; \
  chown -R www-data:www-data www/sites www/modules www/themes; \
  rmdir /var/www/html; \
  ln -sf /opt/drupal/www /var/www/html

ENV PATH=${PATH}:/opt/drupal/vendor/bin
