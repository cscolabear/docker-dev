# ⚠️ NOTICE: This project is outdated and requires maintenance ⚠️
# ⚠️ 注意: 此專案已經過期，需要維護和更新，使用時請注意 ⚠️

This project contains outdated dependencies and configurations:
- PHP 7.3 (End-of-life)
- Outdated PHP extensions
- Node.js v12.17.0 (Outdated)

Please update the dependencies before using in production environments.

---

> preview<br>
![preview](https://user-images.githubusercontent.com/4863629/75411880-c332be00-595b-11ea-8490-aa8389a4636d.png)

# Docker Compose for local development

> - https ready!
> - multiple domain / project support
> - PHP Xdebug
> - php7wrapper
>
> ✨ reference from https://laradock.io

## Index
 - [Setup](#setup)
   - ...
 - [Usage](#usage)
   - [Add Nginx Virtual Host](#add-nginx-virtual-host)
   - [Change PHP Configuration](#change-php-configuration)
 - [Information](#info)
   - ...
- [Xdebug & VSCode](#xdebug--vscode)
- [PHP Wrapper](#php-wrapper)

# Setup
- #### download & install docker APP for MAX(os x)
  > https://docs.docker.com/docker-for-mac/

- #### fork & clone this repository
  >
  >```bash
  >$ git clone https://github.com/cscolabear/docker-dev.git Projects
  >
  > # [optional] create your local branch
  > # e.g. local-dev, and merge any service branch you want(if you need mysql or memcached...)
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
  > ![docker-for-local-dev-folder](https://user-images.githubusercontent.com/4863629/84995568-bd691e80-b17e-11ea-81ca-dcdd7a19e3d6.png)

- #### Build Images & Container
  > ```bash
  > $ cd ~/DockerDev
  > $ chmod -R 755 Logs
  > $ dokcer-compose --compatibility up -d
  > ```
  > (ℹ️ Compatibility mode: https://docs.docker.com/compose/compose-file/compose-versioning/#compatibility-mode)
  >
  > ⏳ Please wait
  >

- #### SSH into a container
  > ```bash
  > $ cd ~/DockerDev/ && clear && docker-compose exec WORKSPACE bash
  > ```
  >
  > looked like after connected~
  > ![docker-for-local-dev-ssh-into](https://user-images.githubusercontent.com/4863629/84996092-66177e00-b17f-11ea-84e9-8f66c95cc194.png)
  > you can execute php, composer, node, npm in this container

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

  - > add local domain:
    >
    > modify `/etc/hosts` (for mac / os x)<br>`C:\WINDOWS\system32\drivers\etc\hosts` (for windows 10)
    >
    > add domain at the bottom
    >
    > e.g.
    > ```conf
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
    > $ docker-compose restart NGINX
    >```

- #### Change PHP Configuration
  > - modify `Dockerfiles/php-fpm/php.ini`
  > - restart php-fpm Docker Container
  > ```base
  > $ docker-compose restart FPM
  >```

## Info
- #### What's in there?
  > - nginx
  >   - fholzer/nginx-brotli:v1.18.0
  >   - https, http2
  >   - logrotate
  > - php-fpm
  >   - php:7.3-fpm
  > - workspace
  >   - node v12.17.0
  >   - npm 6.14.4

- #### `~/DockerDev/Logs/` - nginx log file path
  > e.g.
  >
  > ~/DockerDev/Logs/error.log
  >
  > ~/DockerDev/Logs/nginx-access.log
  >
  > ~/DockerDev/Logs/nginx-error.log

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
  >                 "/var/www": "${workspaceRoot}"
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

  - vscode debug 設定檔每個專案都要設定一次<br>
    - e.g. vscode 打開的專案目錄名稱為 `my-project`，${workspaceRoot} 即為 /var/www/my-project<br>
    所以設定應該是： "/var/www/my-project": "${workspaceRoot}"

    - 路徑對應是重點(pathMappings), 編輯器沒反應時(e.g. vscode) 多半是這個路徑沒有設定正確<br>
    也可以 ssh 進入 fpm container 查看 `/tmp/xdebug.log`<br>
    ```
     "pathMappings": {
         "/var/www": "${workspaceRoot}"
         // or
         "/var/www/[your Project folder]": "${workspaceRoot}"
     },
    ```

     - xdebug.ini
     ; for MAC os x
     xdebug.remote_host=host.docker.internal


# PHP Wrapper

 因為本機 MAC os x 和 docker 環境內的 PHP 版本不同
 所以利用 `phpWrapper` 曝光 docker 內的 php 讓本機開發與執行使用同樣的 php 版本

 ref. https://cola.workxplay.net/docker-php-wrapper-for-local-machine/

 ```bash
 $ ln -s ~/DockerDev/phpWrapper /usr/local/bin php
 $ chmod +x /usr/local/bin php
 ```
 (MAC osx 原生 PHP path: /usr/bin/php)


