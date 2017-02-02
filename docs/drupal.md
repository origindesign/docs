# Drupal & Tools

This covers Drupal 8 and development tools related to it.

## Requirements
- Composer
- Git
- Docker
- Docker Compose

## Setting Up Drupal on Docker

This setup is done on windows, the main projects directory is as close as the C folder to avoid path length limit to 260 characters on Windows. In the following steps, the projects folder is C:\webroot\ and the project name is called "Origin Drop".

### 1. Setting up docker containers

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

### 2. Installing Drupal

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

### 3. Architecture

```
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
```

Note about root directory and file architecture

## Git

Note about what to ignore in git repo, and which git server we use (beanstalk or github)

## Drush

Note about Drush

## Composer

Note about composer

## Hosting

Talk about Pantheon

## Troubleshooting
- If you use Mintty as a terminal emulator for Cygwin, you may have some issues when trying to ssh into docker containers. Prefered solution it to use default cmd for Cygwin or git bash if you prefer not to use Cygwin. See the discusion (here)[https://github.com/docker/docker/pull/22956]

## References
- Based on (docker4drupal)[https://github.com/Wodby/docker4drupal]


