FROM fholzer/nginx-brotli:v1.18.0



RUN set -xe; \
    apk update \
    && apk upgrade \
    && apk add --no-cache openssl bash logrotate \
    && addgroup -g 82 -S www-data ; \
    adduser -u 82 -D -S -G www-data www-data && exit 0 ; exit 1

RUN echo "PS1='\u@\h:\w\$ '" >> ~/.bashrc && \
    echo "" >> ~/.bashrc && \
    rm -rf /etc/nginx/conf.d && \
    # generate SSL certificate
    mkdir /etc/nginx/ssl && \
    openssl genrsa -out "/etc/nginx/ssl/default.key" 2048 && \
    openssl req -new -key "/etc/nginx/ssl/default.key" -out "/etc/nginx/ssl/default.csr" -subj "/CN=default/O=default/C=UK" && \
    openssl x509 -req -days 365 -in "/etc/nginx/ssl/default.csr" -signkey "/etc/nginx/ssl/default.key" -out "/etc/nginx/ssl/default.crt" \
    ;



COPY nginx.logrotate /etc/logrotate.d/nginx
COPY sites.logrotate /etc/logrotate.d/sites
COPY nginx.conf /etc/nginx/



#
# Clean up
#

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /tmp/pear && \
    rm -rf /var/cache/apk/*

# Set default work directory
WORKDIR /var/www


EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
