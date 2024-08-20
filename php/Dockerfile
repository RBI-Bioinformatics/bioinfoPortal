ARG PHP_VERSION="8.1"
FROM php:${PHP_VERSION}-fpm-alpine

RUN apk update && \
    apk upgrade

# Install necessary dependencies
RUN apk add --no-cache \
    libpng-dev \
    libjpeg-turbo-dev \
    libwebp-dev \
    zlib-dev \
    freetype-dev \
    gd \
    postgresql-dev \
    bash \
    libressl \
    ca-certificates \
    openssh-client \
    rsync \
    git \
    curl \
    wget \
    gzip \
    tar \
    patch \
    perl \
    pcre \
    imap \
    imagemagick \
    build-base \
    autoconf \
    libtool \
    pcre-dev \
    imagemagick-dev

# Install PHP extensions
RUN docker-php-ext-configure gd \
    --with-webp \
    --with-jpeg \
    --with-freetype \
    && docker-php-ext-install gd \
    pdo \
    pdo_pgsql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    ln -s /root/.composer/vendor/bin/drush /usr/local/bin/drush

# Install Drush
RUN composer global require drush/drush && \
    composer global update

# Install PHP OPcache
RUN docker-php-ext-install opcache

# Add OPcache configuration
RUN echo "opcache.enable=1" > /usr/local/etc/php/conf.d/opcache.ini && \
    echo "opcache.memory_consumption=128" >> /usr/local/etc/php/conf.d/opcache.ini && \
    echo "opcache.interned_strings_buffer=8" >> /usr/local/etc/php/conf.d/opcache.ini && \
    echo "opcache.max_accelerated_files=4000" >> /usr/local/etc/php/conf.d/opcache.ini && \
    echo "opcache.revalidate_freq=2" >> /usr/local/etc/php/conf.d/opcache.ini && \
    echo "opcache.save_comments=1" >> /usr/local/etc/php/conf.d/opcache.ini

# Install and configure Xdebug (optional)
# RUN pecl install xdebug-3.1.2 && \
#     docker-php-ext-enable xdebug && \
#     echo "zend_extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini && \
#     echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/xdebug.ini && \
#     echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/xdebug.ini

# Xdebug configuration (optional)
# RUN echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/xdebug.ini && \
#     echo "xdebug.client_port=9003" >> /usr/local/etc/php/conf.d/xdebug.ini && \
#     echo "xdebug.log=/tmp/xdebug.log" >> /usr/local/etc/php/conf.d/xdebug.ini

# Clean up
RUN apk del --purge \
    build-base \
    autoconf \
    libtool \
    pcre-dev \
    imagemagick-dev

WORKDIR /var/www/bioinformatics/docroot/tripal4/web

CMD ["php-fpm"]
