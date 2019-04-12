FROM php:7.2-fpm



RUN set -xe; \
    echo "PS1='\u@\h:\w\$ '" >> ~/.bashrc && \
    echo "" >> ~/.bashrc && \
    # Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
    #         "libpng-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
    apt-get update && \
    pecl channel-update pecl.php.net && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    apt-utils \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev

# install zip extensions
# next lines are here becase there is no auto build on docker
RUN apt-get install -y \
    libzip-dev \
    zip \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip

# Install the PHP pdo_mysql extention
RUN docker-php-ext-install pdo_mysql \
    # Install the PHP pdo_pgsql extention
    && docker-php-ext-install pdo_pgsql \
    # Install the PHP gd library
    && docker-php-ext-configure gd \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd



#
# Clean up
#

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /tmp/pear && \
    rm -rf /var/cache/apk/*

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
