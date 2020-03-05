FROM node:10-slim



#
# Clean up
#

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /tmp/pear && \
    rm -rf /var/cache/apk/*

WORKDIR /var/www

EXPOSE 8080
