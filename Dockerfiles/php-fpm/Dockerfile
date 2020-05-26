FROM php:7.3-fpm



RUN set -xe; \
    echo "PS1='\u@\h:\w\$ '" >> ~/.bashrc && \
    echo "" >> ~/.bashrc && \
    apt-get update && \
    pecl channel-update pecl.php.net && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    apt-utils \
    unzip \
    curl \
    libzip-dev \
    zip \
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
    docker-php-ext-install pdo_mysql mysqli \
    # Install the PHP pdo_pgsql extention
    && docker-php-ext-install pdo_pgsql \
    # Install the PHP gd library
    && docker-php-ext-install gd && \
    pecl install xdebug && \
    pecl install memcached-3.1.3 && \
    pecl install redis-4.0.1 && \
    docker-php-ext-enable memcached && \
    docker-php-ext-configure opcache --enable-opcache && \
    docker-php-ext-install opcache && \
    docker-php-ext-enable opcache && \
    docker-php-ext-enable redis && \
    docker-php-ext-enable xdebug && \
    docker-php-ext-configure zip --with-libzip && \
    docker-php-ext-install zip


# install, upgrade openssl
RUN apt-get update -yqq \
 && apt-get install -y --no-install-recommends openssl \
 && sed -i 's,^\(MinProtocol[ ]*=\).*,\1'TLSv1.0',g' /etc/ssl/openssl.cnf \
 && sed -i 's,^\(CipherString[ ]*=\).*,\1'DEFAULT@SECLEVEL=1',g' /etc/ssl/openssl.cnf\
 && rm -rf /var/lib/apt/lists/*



# Install needed php extensions: memcache
RUN curl -fsSL 'https://github.com/websupport-sk/pecl-memcache/archive/NON_BLOCKING_IO_php7.zip' -o memcache.zip \
    && unzip memcache.zip \
    && rm memcache.zip \
    && ( \
    cd pecl-memcache-NON_BLOCKING_IO_php7 \
    && phpize \
    && ./configure --enable-memcache \
    && make \
    && make install \
    ) \
    && rm -r pecl-memcache-NON_BLOCKING_IO_php7 \
    && docker-php-ext-enable memcache



COPY ./pool.conf /usr/local/etc/php-fpm.d/pool.conf



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
