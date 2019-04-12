FROM projects_fpm:latest



ARG USER=developer

ENV TERM xterm
ENV USER ${USER}
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}


USER root
RUN set -xe; \
    apt-get update -yqq && \
    apt-get install -yqq git sudo && \
    # add sudo user
    groupadd -g ${PGID} ${USER} && \
    useradd -u ${PUID} -g ${USER} -m ${USER} -G ${USER} && \
    usermod -p "*" ${USER} -s /bin/bash && \
    usermod -aG sudo ${USER} && \
    sed -e 's/# %wheel ALL=(ALL) NOPASSWD: ALL/%wheel ALL=(ALL) NOPASSWD: ALL/g' -i /etc/sudoers && \
    sed -e "s/^wheel:\(.*\)/wheel:\1,${USER}/g" -i /etc/group



#####################################
# Set Timezone
#####################################

USER root
ARG TZ=UTC
ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone



###########################################################################
# Composer:
###########################################################################

USER root

# Install composer and add its bin to the PATH.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


# Add the composer.json
COPY ./composer.json /home/${USER}/.composer/composer.json
RUN chown -R $USER:$USER /home/$USER && \
    mkdir /home/${USER}/.composer/cache && \
    chmod -R 777 /home/$USER/.composer/cache && \
    composer selfupdate

USER $USER
RUN composer global install && composer clearcache && \
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



###########################################################################
# Node / NVM:
###########################################################################

USER $USER

# Check if NVM needs to be installed
ARG NODE_VERSION='v10.15.3'
ENV NODE_VERSION ${NODE_VERSION}
ENV NVM_DIR /home/${USER}/.nvm

# Install nvm (A Node Version Manager)
RUN mkdir -p $NVM_DIR && \
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash \
    && . $NVM_DIR/nvm.sh && \
    # nvm/v0.34.0/install.sh use NODE_VERSION to install
    # && nvm install ${NODE_VERSION} \
    # && nvm use ${NODE_VERSION} \
    # && nvm alias ${NODE_VERSION} \
    npm install -g gulp && \
    echo "" >> ~/.bashrc

# Add NVM binaries to root's .bashrc
USER root

RUN echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="/home/${USER}/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc && \
    echo "" >> ~/.bashrc



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
