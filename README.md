# Docker Development Environment (Magento2 - Vue.js)
Development environment for existing projects in Magento 2 and Vue.js.
The following repository does not install Magento and Vue.js but allows you to have it on a local environment Docker.

*it is mandatory that magento is installed on a subdirectory and that vue.js also works on a subdirectory.

## Instance list
- apache
- mariadb
- phpmyadmin
- redis
- mailcatcher
- rabbitmq
- elasticsearch
- node

## Before starting
Make sure you have Make GNU and Docker installed on your system.  

### Make GNU
It is possible to install it from:

**Windows**  
Via the package manager [Scoop](https://scoop.sh)

First install scoop by execute:

```
$ Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')

# or shorter
$ iwr -useb get.scoop.sh | iex
```

After installation just do it by execute:

```
$ scoop install make
```

OR

Via the package manager [Chocolatey](https://chocolatey.org)

First install chocolatey by execute:

```
$ Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

After installation just do it by execute:

```
$ choco install make
```

**macOS**

Via the package manager native [Homebrew](https://brew.sh/index_it)

```
$ brew install make
```

**Linux**

```
$ sudo apt install update -y
$ sudo apt install -y make
```

ps: for Linux make should already be present on the system.

### Docker
It is possible to install it from:

**Windows**

From the official website [Docker Desktop on Window](https://docs.docker.com/desktop/windows/install)

**macOS**

From official website [Docker Desktop on Mac](https://docs.docker.com/desktop/mac/install)

**Linux**
```
$ sudo apt install docker.io docker-compose
```

## Initial settings
1. clone this repository to the root of your Magento/Vue project.
2. add to your `.gitignore` file `/.dev`
3. modify the configurations on the .env file (EDIT CUSTOM CONFIGURATIONS)

### Magento
The `/tmp-magento2` folder is temporary which means that after configuring it must be deleted. 

In your Magento directory:
1. get information from **.htaccess.docker** and **env.php.docker** files from `/tmp-magento2`

### Vue
The `/tmp-vue` folder is temporary which means that after configuring it must be deleted.

In your Vue directory:
1. copy the .dev folder to root (vue)
2. get information from **env.docker** and **vue.config.js.docker** files from `/tmp-vue`

## File architecture
Make sure you have similar scaffolding in the **root** project:

```
.dev
  config
    tmp
    .env
    docker-compose.mac.override.yml
    docker-compose.yml
    Makefile
  docker-data
    db
    redis
magento2 (entrypoint magento)
  app
    etc
      env.php.docker -> env.php
    auth.json  
  .htaccess.docker -> .htaccess
vue (entrypoint vue.js)
  .dev
    config
      Dockerfile
  env.docker -> .env
  vue.config.js (if it exists)
```

## Set host into your system (if it hasn't been done)
Mac & Linux

`cd /etc/host/`

Add line

`127.0.0.1    URL.local`

### Running first command
A docker-compose.yml and Makefile is provided inside `.dev/config`
If it is a first installation:

```
$ cd /.dev/config
$ make setup-first
```

After #Raccomandations# then stop and down the containers and restart everything.

```
$ cd /.dev/config
$ make stop
$ make down
```

## Run all containers

```
$ cd /.dev/config
$ make up-verbose
```

AND (the first time or in case of changes)

```
$ make fix-permission
$ make exec-magento CMD='c:c'
$ make exec-magento CMD='c:f'
$ make exec-magento CMD='setup:upgrade'
$ make exec-magento CMD='setup:di:compile'
$ make exec-magento CMD='setup:static-content:deploy -f {store_view}'
$ make exec-magento CMD='index:reset'
$ make exec-magento CMD='index:reindex'
$ make exec-magento CMD='regenerate:product:url'
$ make exec-magento CMD='regenerate:category:url'
```

## Run on the browser
Frontend 

`http://URL.local:3001/{store_view}` `http://URL.local:3001/it_it`

Backoffice

`http://URL.local/backoffice`

## Raccomandations/configurations (Backoffice Magento)
Local email sending settings:
- Enable `Admin->Configuration->Mageplaza Extension->SMTP` module from Backoffice
- Activate the license
- Set the values of `host => mail` and `port => 1025`

Change the configurations of Elastic Search `Admin->Configuration->Catalog->Catalog->Catalog Search`
- Set `Elasticsearch Server Hostname => elasticsearch`
- Set `Elasticsearch Server Port => 9200`
- Set `Elasticsearch Index Prefix => mag_loc`
- Click on Test Connection

## Appendix Command-Line Utilities

#### Start all containers

`$ make up`

#### Start all containers in verbose mode

`$ make up-verbose`

#### Start all containers by forcing build  

`$ make up-build`

#### Stop all containers

`$ make stop`    

#### Down all containers

`$ make down`    

#### Kill all containers

`$ make kill`

#### Open Chrome with disable-web-security (CORS)

`$ make open-chrome`

#### Bash fix-permission commands into container  

`$ make fix-permission`

#### Execute the container apache

`$ make exec-apache`

#### Execute shell magento commands into container

`$ make exec-magento CMD='mycommand'`

`$ make exec-magento CMD='list'` (list all commands)

#### Execute shell n98 commands into container

`$ make exec-n98`

***Before running this command make sure you have installed n98 magerun***

```  
$ make install-n98
```

#### Execute the container db

`$ make exec-db`

### Verify the container phpmyadmin

`http://localhost:8888`

#### Execute the container elasticsearch  

`$ make exec-elasticsearch`

#### Verify is running `istance$ curl localhost:9200/_cat/health`

###Execute the container node  

`$ make exec-node`

#### Execute the container redis  

`$ make exec-redis`

Verify is running `istance$ redis-cli && PING` => output `PONG`

### Execute the container rabbitmq  

`$ make exec-rabbitmq`

Verify is running `http://localhost:15672`

### Verify the container mail

`http://localhost:1080`

#### Upgrade setup
Project dependency update, useful when a composer module has been updated to a certain version  

`$ make setup-upgrade`

## Tools

### xDebug
Make sure you have this lines in the environment settings in the docker-compose file:

```
- PHP_DEBUGGER=xdebug
- XDEBUG_REMOTE_AUTOSTART=1
- XDEBUG_REMOTE_CONNECT_BACK=0
- XDEBUG_PROFILER_ENABLE=1
- XDEBUG_PROFILER_ENABLE_TRIGGER=1000
- XDEBUG_REMOTE_HOST=host.docker.internal
- XDEBUG_REMOTE_PORT=9003
- XDEBUG_IDE_KEY=PHPSTORM # (use your preferred IDE)
```

**macOs**  
For macOs systems make sure you have the ***docker-compose.mac.override.yml***

Useful links:
- [Official site xDebug](https://xdebug.org)
- [Google Extension xDebug](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc?hl=it)
- [VS Code](https://code.visualstudio.com/docs/languages/php)
- [Atom](https://atom.io/packages/php-debug)
- [Eclipse](https://wiki.eclipse.org/Debugging_using_XDebug)

### ElasticSearch
To view the data within the Elasticsearch instance, download the Google Chrome `ElasticSearch Head` extension.

If the data is indexed correctly you should see something like this:

ps: in case of denied access to make commands, run them with `sudo`, on Linux environments it may be necessary to set the current user of the system to the variable 'CURRENT_USER' present in the `/.dev/config/Makefile` file.
