FROM php:7.3-cli

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
    libpq-dev \
    libssl-dev \
    libvpx-dev \
    libxpm-dev \
    libzip-dev \
    pkg-config \
    zlib1g-dev

# Create a custom user to run the container on
RUN useradd php \
 && mkdir -p /home/php \
 && touch /home/php/.profile \
 && chown -R php:php /home/php \
 && mkdir /data \
 && chown -R php:php /data

# Copy over the configuration file for PHP
COPY usr/local/etc/php/php.ini /usr/local/etc/php/php.ini

# The directory to work from
WORKDIR /data

# The volume that can be mounted
VOLUME /data

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
    pdo_pgsql \
    shmop \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm \
    zip \
 && docker-php-ext-enable \
    memcached \
    xdebug

# Run the container as the configured user
USER php:php
