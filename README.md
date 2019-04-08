# Docker Compose for local development

## Index
 - [Setup](#setup)
 - [Usage](#usage)
   - [Add Nginx Virtual Host](#add-nginx-virtual-host)
 - [Information](#information)

# Setup
 - #### download docker for MAX(os x)
   > https://docs.docker.com/docker-for-mac/
 - #### fork & clone this repository
 - #### make your folder look like this... (replace `~/Projects` what you want.)
    > e.g.
    > 
    > ~/Projects
    >
    > ~/Projects/[your project - 1]
    >
    > ~/Projects/[your project - 2]
    >
    > ![docker-for-local-dev-folder](https://user-images.githubusercontent.com/4863629/55706301-74162e80-5a13-11e9-98c6-3101c7e406c7.png)

- #### in command line move to ~/Projects
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
    > when connect into Docker Containers successful you can see~
    > ![docker-for-local-dev-ssh-into](https://user-images.githubusercontent.com/4863629/55706770-99f00300-5a14-11e9-8600-464a3cba62aa.png)

- #### enjoy Docker 🐳
    > create index.html
    >```bash
    > $ echo "<h1>127.0.0.1 - fine</h1>" > index.html
    >```
    > open browser - http://localhost
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


## Info
- #### Services include...
  - Nginx - nginx:alpine
  - NodeJS - alpine-node:10
    - npm 6.8.0
    - yarn 1.13.0

- #### `~/Projects/Logs/` - nginx log file path
    > e.g. 
    > 
    > ~/Projects/Logs/nginx-access.log
    >
    > ~/Projects/Logs/nginx-error.log
    