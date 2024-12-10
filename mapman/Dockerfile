# Use the official Apache image as a base
FROM httpd:alpine

# Copy the MapManJS files to the Apache html directory
COPY ./usadellab.github.io/MapManJS /mapman

# Expose port 80
EXPOSE 80

# Start Apache
CMD ["httpd-foreground"]