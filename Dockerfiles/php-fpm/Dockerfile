FROM php:7.2-fpm



RUN set -xe; \
    echo "PS1='\u@\h:\w\$ '" >> ~/.bashrc && \
    echo "" >> ~/.bashrc && \
    apt-get update && \
    pecl channel-update pecl.php.net && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    apt-utils \
    curl \
    libmemcached-dev \
    zlib1g-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev && \
    # Install the PHP extention
    docker-php-ext-install pdo_mysql \
    # Install the PHP pdo_pgsql extention
    && docker-php-ext-install pdo_pgsql \
    # Install the PHP gd library
    && docker-php-ext-configure gd \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd && \
    pecl install xdebug && \
    pecl install memcached-3.1.3 && \
    docker-php-ext-enable memcached && \
    docker-php-ext-enable xdebug


# install zip extensions
# next lines are here becase there is no auto build on docker
RUN apt-get install -y --no-install-recommends \
    libzip-dev \
    zip \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip


COPY ./pool.conf /usr/local/etc/php-fpm.d/pool.conf
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini



#
# Clean up
#

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /tmp/pear && \
    rm -rf /var/cache/apk/* && \
    docker-php-source delete

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
