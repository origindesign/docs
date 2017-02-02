# Drupal & Tools

This covers Drupal 8 and development tools related to it.

## Requirements
- Composer
- Git
- Docker
- Docker Compose

## Setting Up Drupal on Docker

This setup is done on windows, the main projects directory is as close as the C folder to avoid path length limit to 260 character on Windows. In the following steps, the projects folder is C:\webroot\ and the project name is called "Origin Drop".

### 1. Setting up docker containers

The following command will:
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

## Architecture

Note about root directory and file architecture

## Git

Note about what to ignore in git repo, and which git server we use (beanstalk or github)

## Drush

Note about Drush

## Composer

Note about composer

## Hosting

Talk about Pantheon
