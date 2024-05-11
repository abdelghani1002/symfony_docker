FROM php:8.2-fpm

# Update and install required packages
RUN apt update \
    && apt install -y \
    zip \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
    &&  mv composer.phar /usr/local/bin/composer

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_18.x -o nodesource_setup.sh \
    && bash nodesource_setup.sh \
    && apt install -y nodejs \
    && rm -rf /var/lib/apt/lists/*

# Install ICU libraries, development files and Git
RUN apt update \
    && apt install -y \
    libicu-dev \
    git \
    && rm -rf /var/lib/apt/lists/*

# Install Nginx
RUN apt update \
    && apt install -y \
    nginx \
    && rm -rf /var/lib/apt/lists/*

# Copy Nginx configuration
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf

# Set the working directory
WORKDIR /var/www

# Expose port 9000 to communicate with PHP-FPM
EXPOSE 9000

# Create a script to start Nginx and PHP-FPM
RUN echo "#!/bin/bash" > /start.sh \
    && echo "service nginx start" >> /start.sh \
    && echo "php-fpm" >> /start.sh \
    && chmod +x /start.sh

# Start Nginx and PHP-FPM
CMD ["/start.sh"]
