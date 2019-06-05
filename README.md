# Docker Compose for local development

> - https ready!
> - multiple domain / project support
> - PHP Xdebug
>
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
- [Xdebug & VSCode](#xdebug--vscode)

# Setup
- #### download & install docker for MAX(os x)
  > https://docs.docker.com/docker-for-mac/

- #### fork & clone this repository
  >
  >```bash
  >$ git clone https://github.com/cscolabear/docker-dev.git Projects
  >
  > # [optional] change branch, if you need mysql
  > $ git checkout service/add-mysql
  > ```
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

- #### Build Images & Container
  > ```bash
  > $ cd ~/Projects
  > $ chmod -R 755 Logs
  > $ dokcer-compose up -d
  > ```
  >
  > ⏳ Please wait

- #### SSH into a container
  > ```bash
  > $ cd ~/Projects/ && clear && docker-compose exec workspace bash
  > ```
  >
  > looked like after connected~
  > ![docker-for-local-dev-ssh-into](https://user-images.githubusercontent.com/4863629/56189375-60457a80-605a-11e9-9e6d-7a948d339a4c.png)
  > you can execute composer, node, npm in here

- #### enjoy Docker 🐳
  > 👉 create index.php & index.html
  > in `~/Projects`
  >
  > use command~
  >```bash
  > $ echo -e "<?php\nphpinfo();" > index.php
  > $ echo "<h1>127.0.0.1 - fine</h1>" > index.html
  >```
  > open browser - http://localhost or https://localhost
  >
  >![docker-localhost-idnex-php](https://user-images.githubusercontent.com/4863629/58859932-8348ee00-86dd-11e9-998e-a9b83d3ef4b8.png)
  >
  > index.html > https://localhost/index.html)
  >![docker-for-local-dev-ssh-localhost](https://user-images.githubusercontent.com/4863629/58859819-4e3c9b80-86dd-11e9-8411-85553d556e3c.png)


## Usage
- #### Add Nginx Virtual Host
  - > creating a new vhost.conf file in `~/Projects/Dockerfiles/nginx/sites-enabled/`
    >
    > nginx conf tools: https://nginxconfig.io

  - > modify `/etc/hosts` (mac / os x)<br>`C:\WINDOWS\system32\drivers\etc\hosts` (windows 10)
    >
    > add domain at the bottom
    > ```
    > 127.0.0.1    local.[any_you_want_domain_1]
    > 127.0.0.1    local.[any_you_want_domain_2]
    >```

  - > add host at the `docker-compose.yml`
    >
    > add host domain at, `services > fpm > extra_hosts`
    >
    > add new line <br>
    > ```
    > - "local.[any_you_want_domain_1]:172.16.1.50"
    > ```
    >
    > looks like this
    > ![host-compose-yml](https://user-images.githubusercontent.com/4863629/58926136-a37bba00-877c-11e9-8895-67a4f7cb20f8.png)
    >

  - > restart nginx Docker Container
    >```bash
    > $ docker-compose restart nginx
    >```

- #### Change PHP Configuration
  > - modify `Dockerfiles/php-fpm/php.ini`
  > - restart php-fpm Docker Container
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
  > ![docker-image-size](https://user-images.githubusercontent.com/4863629/58860143-0d915200-86de-11e9-9b5a-d4cf688a230d.png)



- #### Xdebug & VSCode
  > - install the vscode extension <br>
  > - PHP Debug
  > https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug
  >
  > - in vscoe toolsbar `Debug > Add Configuration > open & modify launch.json`
  >
  > ```json
  > {
  >     "version": "0.2.0",
  >     "configurations": [
  >         {
  >             "name": "Listen for > XDebug",
  >             "type": "php",
  >             "request": "launch",
  >             "port": 9001,
  >             "pathMappings": {
  >                 "/var/www": "$> {workspaceRoot}"
  >             },
  >             "log": true
  >         },
  >         {
  >             "name": "Launch > currently open script",
  >             "type": "php",
  >             "request": "launch",
  >             "program": "${file}",
  >             "cwd": "${fileDirname}",
  >             "port": 9001
  >         }
  >     ]
  > }
  > ```