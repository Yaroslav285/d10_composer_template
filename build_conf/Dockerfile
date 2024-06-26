# Deployable PHP image
FROM php:8.2-apache-bullseye

ARG SSH_CREDS='test'

# install the PHP extensions we need
RUN set -eux; \
  \
  apt-get update; \
  apt-get install -y apt-transport-https gnupg cron default-mysql-client libjpeg-turbo-progs; \
  apt-get install -y --no-install-recommends \
  libfreetype6-dev \
  optipng \
  advancecomp \
  libjpeg-dev \
  libwebp-dev \
  libpng-dev \
  libpq-dev \
  libzip-dev \
  ; \
  \
  docker-php-ext-configure gd \
  --with-freetype \
  --with-jpeg=/usr \
  --with-webp \
  ; \
  \
  pecl install uploadprogress; \
  pecl install apcu; \
  docker-php-ext-enable apcu; \
  docker-php-ext-enable uploadprogress; \
  docker-php-ext-install -j "$(nproc)" \
  bcmath \
  gd \
  exif \
  opcache \
  pdo_mysql \
  zip \
  ;

RUN apt-get update \
    && apt-get install -y --no-install-recommends dialog \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo ${SSH_CREDS} | chpasswd;

WORKDIR /opt/drupal

COPY . /opt/drupal
COPY build_conf/sshd_config /etc/ssh/sshd_config

RUN set -eux; \
  chown -R www-data:www-data web/sites web/modules web/themes; \
  rmdir /var/www/html; \
  ln -sf /opt/drupal/web /var/www/html

EXPOSE 80 2222

ENV PATH=${PATH}:/opt/drupal/vendor/bin

RUN mkdir -p -m0755 /var/run/sshd

CMD [ "sh", "-c", "/usr/sbin/sshd && apache2-foreground" ]
