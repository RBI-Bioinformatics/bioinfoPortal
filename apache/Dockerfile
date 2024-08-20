ARG APACHE_VERSION="2.4.52"
FROM httpd:${APACHE_VERSION:+${APACHE_VERSION}-}alpine

RUN apk update && apk upgrade

# Copy apache vhost file to proxy php requests to php-fpm container
COPY local.apache.conf /usr/local/apache2/conf/local.apache.conf

# Set proper permissions for document root
RUN mkdir -p /var/www/bioinformatics/docroot/tripal4/web && \
    chown -R www-data:www-data /var/www/bioinformatics/docroot/tripal4/web && \
    chmod -R 755 /var/www/bioinformatics/docroot/tripal4/web

# Include the configuration file
RUN echo "Include /usr/local/apache2/conf/local.apache.conf" >> /usr/local/apache2/conf/httpd.conf

# Make sites/default/files writable
RUN mkdir -p /var/www/bioinformatics/docroot/tripal4/web/sites/default/files && \
    chown -R www-data:www-data /var/www/bioinformatics/docroot/tripal4/web/sites/default/files && \
    chmod -R 755 /var/www/bioinformatics/docroot/tripal4/web/sites/default/files