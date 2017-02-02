# Drupal & Tools

This covers Drupal 8 and development tools related to it.

## Requirements
- Composer
- Git
- Docker
- Docker Compose

## Setting Up Drupal

In your projects folder:
```shell
mkdir my-site
cd my-site
git clone https://github.com/drupal-composer/drupal-project.git
mv drupal-project/* .
rm -r drupal-project -y
```

This will grab a composer template from [drupal-project](https://github.com/drupal-composer/drupal-project), install it into drupal-project folder, moving the necessary files into your my-site folder and delete the rest. 


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
