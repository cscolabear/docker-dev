FROM node:10-slim


RUN npm install pm2 -g



#
# Clean up
#

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /tmp/pear && \
    rm -rf /var/cache/apk/*

WORKDIR /var/www

EXPOSE 3000

CMD ["pm2-docker", "/pm2-process.yml"]
