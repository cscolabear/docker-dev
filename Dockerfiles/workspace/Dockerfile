FROM cscolabear/swoole-fpm-7.3-fpm:0.1



ARG USER=developer

ENV TERM xterm
ENV USER ${USER}
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

ARG TZ=UTC
ENV TZ ${TZ}


USER root
RUN set -xe; \
    apt-get update -yqq && \
    apt-get install -yqq --no-install-recommends git sudo && \
    # add sudo user
    groupadd -g ${PGID} ${USER} && \
    useradd -u ${PUID} -g ${USER} -m ${USER} -G ${USER} && \
    usermod -p "*" ${USER} -s /bin/bash && \
    usermod -aG sudo ${USER} && \
    echo "${USER} ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/${USER} && \
    chmod 0440 /etc/sudoers.d/${USER} && \
    # Set Timezone
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone



###########################################################################
# Composer:
###########################################################################

USER root

# Install composer and add its bin to the PATH.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    chown -R $USER:$USER /home/$USER && \
    mkdir -p /home/${USER}/.composer/cache && \
    chown -R $USER:$USER /home/$USER/.composer && \
    chmod -R 755 /home/$USER/.composer && \
    composer selfupdate

# Add the composer.json
COPY ./composer.json /home/${USER}/.composer/composer.json

USER $USER
RUN composer clear-cache && composer global install && composer clearcache && \
    # Export composer vendor path
    echo "" >> ~/.bashrc && \
    echo 'export PATH="~/.composer/vendor/bin:$PATH"' >> ~/.bashrc



#####################################
# User Aliases
#####################################

USER $USER

COPY ./aliases.sh /home/${USER}/aliases.sh
RUN echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source /home/${USER}/aliases.sh" >> ~/.bashrc && \
    echo "" >> ~/.bashrc

USER root
RUN echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source /home/${USER}/aliases.sh" >> ~/.bashrc && \
    echo "" >> ~/.bashrc



# node

ENV NODE_VERSION 12.17.0

USER root

RUN groupadd --gid 1001 node \
    && useradd --uid 1001 --gid node --shell /bin/bash --create-home node && \
    # https://github.com/GoogleChrome/puppeteer
    buildDeps='xz-utils' \
    && ARCH= && dpkgArch="$(dpkg --print-architecture)" \
    && case "${dpkgArch##*-}" in \
    amd64) ARCH='x64';; \
    ppc64el) ARCH='ppc64le';; \
    s390x) ARCH='s390x';; \
    arm64) ARCH='arm64';; \
    armhf) ARCH='armv7l';; \
    i386) ARCH='x86';; \
    *) echo "unsupported architecture"; exit 1 ;; \
    esac \
    && set -ex \
    && apt-get update && apt-get install -y ca-certificates curl wget gnupg dirmngr $buildDeps --no-install-recommends \
    && for key in \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    FD3A5288F042B6850C66B31F09FE44734EB7990E \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
    B9AE9905FFD7803F25714661B63B535A4C206CA9 \
    77984A986EBC2AA786BC0F66B01FBB92821C587A \
    8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
    4ED778F539E3634C779C87C6D7062848A1AB005C \
    A48C2BEE680E841632CD4E44F07496B3EB3C1762 \
    B9E2F5981AA6E0CD28160D9FF13993A75599653C \
    ; do \
    gpg --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
    gpg --batch --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --batch --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
    done \
    && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-$ARCH.tar.xz" \
    && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
    && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
    && grep " node-v$NODE_VERSION-linux-$ARCH.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
    && tar -xJf "node-v$NODE_VERSION-linux-$ARCH.tar.xz" -C /usr/local --strip-components=1 --no-same-owner \
    && rm "node-v$NODE_VERSION-linux-$ARCH.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
    && apt-get purge -y --auto-remove $buildDeps \
    && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
    # install Chromium
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-unstable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst \
    --no-install-recommends



#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

USER root

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /var/cache/apk/* && \
    rm -rf /var/log/lastlog /var/log/faillog

USER $USER

# Set default work directory
WORKDIR /var/www
