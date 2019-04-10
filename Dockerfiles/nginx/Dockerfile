FROM nginx:alpine



COPY nginx.logrotate /etc/logrotate.d/nginx
COPY sites.logrotate /etc/logrotate.d/sites
COPY nginx.conf /etc/nginx/

RUN rm /etc/nginx/conf.d/default.conf && \
    mkdir /etc/nginx/sites-enabled

RUN apk update \
    && apk upgrade \
    && apk add --no-cache openssl \
    && apk add --no-cache bash \
    && adduser -D -H -u 1000 -s /bin/bash www-data && \
    echo "PS1='\u@\h:\w\$ '" >> ~/.bashrc && \
    echo "" >> ~/.bashrc



# generate SSL certificate
RUN if [ ! -f /etc/nginx/ssl/default.crt ] ; then \
        mkdir /etc/nginx/ssl && \
        openssl genrsa -out "/etc/nginx/ssl/default.key" 2048 && \
        openssl req -new -key "/etc/nginx/ssl/default.key" -out "/etc/nginx/ssl/default.csr" -subj "/CN=default/O=default/C=UK" && \
        openssl x509 -req -days 365 -in "/etc/nginx/ssl/default.csr" -signkey "/etc/nginx/ssl/default.key" -out "/etc/nginx/ssl/default.crt" \
        ; \
    else \
        echo "already has crt" ; \
    fi



#
# Clean up
#

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /tmp/pear && \
    rm -rf /var/cache/apk/*

# Set default work directory
WORKDIR /var/www/html


EXPOSE 80 443
