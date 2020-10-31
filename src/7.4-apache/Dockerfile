FROM php:7.4-apache

# The user and group id to assign to the user
ARG USER_ID=1000
ARG GROUP_ID=1000

# The domain name used for the SSL certificate
ENV DOMAIN_NAME=www.example.com

# Configure the users
RUN usermod -u $USER_ID www-data \
 && groupmod -g $GROUP_ID www-data

# Add the virtual host
COPY etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf

# Install and configure Apache
RUN a2enmod rewrite \
 && a2enmod ssl \
 && echo "AllowEncodedSlashes On" >> /etc/apache2/apache2.conf

# Configure the system
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    wget \
    git \
    libbz2-dev \
    libfreetype6-dev \
    libgmp-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmemcached-dev \
    libpng-dev \
    libssl-dev \
    libvpx-dev \
    libxpm-dev \
    libzip-dev \
    pkg-config \
    zlib1g-dev \
 && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h

# Install PECL extensions
RUN pecl channel-update pecl.php.net \
 && pecl install memcached xdebug

# Install PHP extensions
RUN docker-php-ext-configure intl \
 && docker-php-ext-configure gmp --with-gmp=/usr/include/x86_64-linux-gnu \
 && docker-php-ext-configure gd \
    --with-freetype-dir=/usr/lib/x86_64-linux-gnu/ \
    --with-jpeg-dir=/usr/lib/x86_64-linux-gnu/ \
    --with-xpm-dir=/usr/lib/x86_64-linux-gnu/ \
 && docker-php-ext-install \
    bcmath \
    bz2 \
    exif \
    ftp \
    gd \
    gettext \
    intl \
    mbstring \
    mysqli \
    opcache \
    pdo_mysql \
    shmop \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm \
    zip \
 && docker-php-ext-enable \
    memcached \
    xdebug

# The new script to copy, needed since we generate an SSL certificate in it
COPY usr/local/bin/apache2-foreground /usr/local/bin/
