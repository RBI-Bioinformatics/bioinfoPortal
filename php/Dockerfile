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
    jq curl \
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

# Install blast+ for syntenic viewer. Example URL, replace with the actual latest version
# RUN wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.16.0+-x64-linux.tar.gz && \
#     tar -xzf ncbi-blast-2.16.0+-x64-linux.tar.gz && \
#     mv ncbi-blast-2.16.0+/bin/* /usr/local/bin/ && \
#     rm -rf ncbi-blast-2.16.0+-x64-linux.tar.gz ncbi-blast-2.16.0+

# Increase PHP memory limit and upload settings
RUN echo "memory_limit=2G" > /usr/local/etc/php/conf.d/memory-limit.ini && \
    echo "upload_max_filesize=2G" >> /usr/local/etc/php/conf.d/uploads.ini && \
    echo "max_input_time=300" >> /usr/local/etc/php/conf.d/uploads.ini && \
    echo "max_execution_time=300" >> /usr/local/etc/php/conf.d/uploads.ini && \
    echo "post_max_size=2G" >> /usr/local/etc/php/conf.d/uploads.ini

# Clean up
RUN apk del --purge \
    build-base \
    autoconf \
    libtool \
    pcre-dev \
    imagemagick-dev

WORKDIR /var/www/bioinformatics/docroot/tripal4/web

CMD ["php-fpm"]