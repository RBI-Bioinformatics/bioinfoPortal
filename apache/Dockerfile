# Use the PHP-Apache image
ARG PHP_VERSION="8.1"
FROM php:${PHP_VERSION}-apache

# Update system packages
RUN apt-get update && apt-get upgrade -y

# Install necessary dependencies
RUN apt-get install -y --no-install-recommends \
    libpng-dev \
    libjpeg-dev \
    libwebp-dev \
    zlib1g-dev \
    libfreetype6-dev \
    libpq-dev \
    bash \
    openssl \
    ca-certificates \
    openssh-client \
    rsync \
    git \
    jq \
    curl \
    wget \
    gzip \
    tar \
    patch \
    perl \
    libpcre3-dev \
    imagemagick \
    libmagickwand-dev \
    build-essential \
    autoconf \
    libtool \
    libpcre3 \
    libjpeg62-turbo-dev \
    libfreetype6

# Install PHP extensions
RUN docker-php-ext-configure gd --with-jpeg --with-webp --with-freetype && \
    docker-php-ext-install gd pdo pdo_pgsql

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

# Increase PHP memory limit and upload settings
RUN echo "memory_limit=2G" > /usr/local/etc/php/conf.d/memory-limit.ini && \
    echo "upload_max_filesize=2G" >> /usr/local/etc/php/conf.d/uploads.ini && \
    echo "max_input_time=300" >> /usr/local/etc/php/conf.d/uploads.ini && \
    echo "max_execution_time=300" >> /usr/local/etc/php/conf.d/uploads.ini && \
    echo "post_max_size=2G" >> /usr/local/etc/php/conf.d/uploads.ini

# Update DocumentRoot in apache2.conf and 000-default.conf
RUN sed -i 's|/var/www/html|/var/www/docroot/web|g' /etc/apache2/apache2.conf
RUN sed -i 's|/var/www/html|/var/www/docroot/web|g' /etc/apache2/sites-enabled/000-default.conf

# Set the working directory
WORKDIR /var/www/docroot

# # Ensures that the sites/default/files directory has the correct permissions.
# RUN mkdir -p /var/www/docroot/sites/default/files && \
#     chmod -R 777 /var/www/docroot/sites/default/files

# Expose port 80 for Apache
EXPOSE 80

# Start Apache server
CMD ["apache2-foreground"]