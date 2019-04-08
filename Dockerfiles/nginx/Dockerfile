FROM nginx:alpine



ADD nginx.logrotate /etc/logrotate.d/nginx
ADD sites.logrotate /etc/logrotate.d/sites
ADD nginx.conf /etc/nginx/

RUN rm /etc/nginx/conf.d/default.conf && \
    mkdir /etc/nginx/sites-enabled

RUN apk update \
    && apk upgrade \
    && apk add --no-cache openssl \
    && apk add --no-cache bash \
    && adduser -D -H -u 1000 -s /bin/bash www-data



#
# Clean up
#

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /tmp/pear && \
    rm -rf /var/cache/apk/*

# Set default work directory
WORKDIR /var/www/html


EXPOSE 80 443
