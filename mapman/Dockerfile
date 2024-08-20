# Use the official Apache image as a base
FROM httpd:alpine

# Copy the MapManJS files to the Apache html directory
COPY ./usadellab.github.io/MapManJS /usr/local/apache2/htdocs/

# Set proper permissions for htdocs directory and its contents
RUN chown -R www-data:www-data /usr/local/apache2/htdocs/ && \
    chmod -R 755 /usr/local/apache2/htdocs/

# Set the ServerName to localhost to avoid FQDN warning
RUN echo "ServerName localhost" >> /usr/local/apache2/conf/httpd.conf

# Expose port 80
EXPOSE 80

# Start Apache
CMD ["httpd-foreground"]