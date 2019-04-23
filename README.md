# Docker Compose for local development

> reference from https://laradock.io

## Index
 - [Setup](#setup)
   - ...
 - [Usage](#usage)
   - [Add Nginx Virtual Host](#add-nginx-virtual-host)
   - [Change PHP Configuration](#change-php-configuration)
 - [Information](#info)
   - ...
   - [docker image size preview](#docker-image-size)

# Setup
 - #### download docker for MAX(os x)
   > https://docs.docker.com/docker-for-mac/
 - #### fork & clone this repository
 - #### make your folder look like this... (👉 replace `~/Projects` what you want.)
    > e.g.
    >
    > ~/Projects
    >
    > ~/Projects/[your project - 1]
    >
    > ~/Projects/[your project - 2]
    >
    > ![docker-for-local-dev-folder](https://user-images.githubusercontent.com/4863629/55706301-74162e80-5a13-11e9-98c6-3101c7e406c7.png)

- #### in command line move to `~/Projects`
    > ```bash
    > $ chmod -R 755 Logs
    > $ dokcer-compose up -d
    > ```
    >
    > ⏳ wait for download & build docker image, container

- #### SSH into a container
    > ```bash
    > $ cd ~/Projects/ && clear && docker-compose exec workspace bash
    > ```
    >
    > when connect into Docker Containers successful you can see~
    > ![docker-for-local-dev-ssh-into](https://user-images.githubusercontent.com/4863629/56189375-60457a80-605a-11e9-9e6d-7a948d339a4c.png)
    > you can execute composer, node, npm in this container

- #### enjoy Docker 🐳
    > 👉 create index.php & index.html
    > in `~/Projects`
    >```bash
    > $ echo -e "<?php\nphpinfo();" > index.php
    > $ echo "<h1>127.0.0.1 - fine</h1>" > index.html
    >```
    > open browser - http://localhost or https://localhost
    >
    >![docker-for-local-dev-ssh-localhost](https://user-images.githubusercontent.com/4863629/55710384-1ab2fd00-5a1d-11e9-9b33-dbc6e429b63c.png)


## Usage
 - #### Add Nginx Virtual Host
   - >  creating a new vhost.conf file in ~/Projects/Dockerfiles/nginx/sites-enabled/
     >
     > ref. https://nginxconfig.io

   - restart nginx Docker Container
     > ```base
     > $ docker-compose restart nginx
     >```

 - #### Change PHP Configuration
   - > modify `Dockerfiles/php-fpm/php.ini`
   - restart php-fpm Docker Container
     > ```base
     > $ docker-compose restart fpm
     >```

## Info
- #### What's in there?
  > - nginx
  >   - nginx:alpine
  >   - https, http2
  >   - logrotate
  > - php-fpm
  >   - php:7.2-fpm
  > - workspace
  >   - nvm 0.34.0
  >   - node v10.15.3
  >   - npm 6.4.1

- #### `~/Projects/Logs/` - nginx log file path
  > e.g.
  >
  > ~/Projects/Logs/error.log
  >
  > ~/Projects/Logs/nginx-access.log
  >
  > ~/Projects/Logs/nginx-error.log

- #### docker image size
  > ![docker-image-size](https://user-images.githubusercontent.com/4863629/56190582-18742280-605d-11e9-9332-dc32ef997b94.png)

