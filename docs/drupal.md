# Drupal & Tools

This covers Drupal 8 and development tools related to it.

## Requirements
- Composer
- Git
- Docker
- Docker Compose

## 1. Setting Up Drupal on Docker

This setup is done on windows, the main projects directory is as close as the C folder to avoid path length limit to 260 characters on Windows. In the following steps, the projects folder is C:\webroot\ and the project name is called "Origin Drop".

### a. Setting up docker containers

The following commands will:

- Navigate to the webroot directory
- Download the docker setup from Origin Docker repository into your new project directory.
- Install docker containers locally (a LEMP stack working with Nginx, Php 7, MariaDB, PhpMyAdmin and mailhog)
- Check everything is working fine
```shell
cd /webroot
git clone git@github.com:origindesign/origin-docker.git origindrop
cd origindrop
docker-compose up -d
docker-compose ps
```
This should list all the Docker Container running. Make sure their states are all up.

### b. Installing Drupal

The following commands will:

- Connect to the php docker container using ssh
- Install Drupal 8 into webroot/origindrop/site/ folder, using composer from the [Drupal Composer project](https://github.com/drupal-composer/drupal-project)

*Note that the PHP container has composer and drush pre-installed*

```shell
docker-compose exec --user 82 php sh
composer create-project drupal-composer/drupal-project:8.x-dev /var/www/html --stability dev --no-interaction
```
- Navigate to <http://localhost:8000> and you should see the Drupal Installation page
- Navigate to <http://localhost:8001> and you should see PhpMyAdmin interface with an empty drupal database
- From the install page, follow the classic Drupal installation step using the following credentials:
```
Database: drupal
Username: drupal
Password: drupal
host: mariadb
port: 3306
```
- After the installation, enter the site information and you should get your fresh Drupal 8 Site.
- At the time of this writing, docker has some permissions issues in the files directory with drupal 8.2.x version in the core/includes/files.inc. This can be solved by using drupal > 8.3.x (alpha at the time of this writing) and applying this [patch](https://www.drupal.org/node/944582)

### c. Architecture
<pre>
webroot/
└── origindrop/
    ├── site/
    |   ├── drush/
    |   |   └── ...
    |   ├── scripts/
    |   |   └── ...
    |   ├── vendor/
    |   |   └── ...
    |   ├── web/
    |   |   ├── core/
    |   |   ├── modules/
    |   |   ├── profiles/
    |   |   ├── sites/
    |   |   ├── themes/
    |   |   ├── .htaccess
    |   |   ├── autoload.php
    |   |   ├── index.php
    |   |   ├── robots.txt
    |   |   ├── update.php
    |   |   ├── web.config
    |   |   └── ...
    |   ├── .gitignore
    |   ├── .travis.yml
    |   ├── composer.json
    |   ├── composer.lock
    |   ├── LICENCE
    |   ├── phpunit.xml.dist
    |   └── README.md
    ├── docker-runtime
    |   └── ...
    └── docker-compose.yml
</pre>

### d. Stopping and Removing containers

IMPORTANT: Do not use `docker-compose down` command because it will purge MariaDB volume. Instead use `docker-compose stop`. If you restart Docker you WILL NOT lose your MariaDB data. 

## 2. Using composer to manage modules

Note about composer

## 3. Using Drush to manage Config Manager and Cache Rebuild

- Drush can be access normally after sshing into the php container:
```
docker-compose exec php sh
cd var/www/html/web
drush status
```
- Drush can aslo be accessed through the docker-compose command and by specifiying the root directory `docker-compose exec php drush -r /var/www/html/web/ status`
- In order to simplify the command, you can create an alias in your .bashrc file like `alias ddrush='docker-compose exec php drush'` (I called mine "ddrush" for docker drush)
- Then you'd need to create your drush aliases and copy it from your local machine to the php container drush directory:
```
cd ~/.drush
docker cp pantheon.aliases.drushrc.php origindrop_php_1:/root/.drush/pantheon.aliases.drushrc.php
```
- Depending on the name you set for your aliases, you should be able to run drush from your local like these:
```
ddrush @local status 
ddrush @pantheon.origindrop.dev status 
```


## 4. Pushing into Host (Pantheon)

- There might be some drush conflict when pushing from your local to pantheon directly. The recommended solution is to use the "per project drush" way which embed drush in the vendor directory through composer. By default it's added with the drupal-composer upstream. However, if it's not working, it might be necessary to use the global drush packaged in by Pantheon hosting. In order to do that, you can simply remove drush from your composer.json by doing `compose remove drush/drush`.

## Troubleshooting
- If you use Mintty as a terminal emulator for Cygwin, you may have some issues when trying to ssh into docker containers. Prefered solution it to use default cmd for Cygwin or git bash if you prefer not to use Cygwin. See the discusion [here](https://github.com/docker/docker/pull/22956)

## References
- Based on [docker4drupal](https://github.com/Wodby/docker4drupal)


