# Drupal & Tools

This covers Drupal 8 and development tools related to it.

## Requirements
- Composer
- Git
- Docker
- Docker Compose

## Setting Up Drupal

1. Navigate to your folder where you store all your projects
```shell
cd .../projects
composer create-project drupal-composer/drupal-project:8.x-dev my-site --stability dev --no-interaction
```
This will create a drupal site organized as per [drupal-project](https://github.com/drupal-composer/drupal-project) in the projects/my-site folder.

2. Download the Docker setup from Origin Design Github account and store it into your project
```shell
cd my-site
curl -o docker-compose.yml https://raw.githubusercontent.com/origindesign/origin-docker/master/docker-compose.yml
```

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
