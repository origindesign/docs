# Docker for Windows

## Local Directory

- Create a folder 'c://webroot' to store all local websites. This is important to avoid issues with paths that get too long


## Adding shortcuts to .bashrc
````
alias ddrush='docker-compose exec php drush'
alias dcomposer='docker-compose exec php composer'
alias dc='docker-compose'
````

## List of useful docker commands

- Creating containers for the first time from a docker-compose file
```
$ docker-compose up -d
```

- Starting an existing set of container
```
$ docker-compose start
```

- Listing local containers
```
$ docker-compose ps
```

- Listing all docker containers on the machine
```
$ docker ps -a
```

- Stopping an existing set of container
```
$ docker-compose stop
```

- Removing an existing set of container (to use with caution as it will remove database and file added outside /var/www/html/)
```
$ docker-compose down
```

- Connecting to a container via SSH
```
$ docker-compose exec php sh
```

- Copying files from local to php container inside root folder:
```
$ docker cp filename.ext name_of_php_container:/root/filename.ext
```

- Importing a database into mariadb container
```
$ docker exec - name_of_mariadb_container mysql -h mariadb -u drupal -p drupal drupal --force < database_filename.sql
```
