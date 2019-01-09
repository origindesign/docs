# Pantheon

## Pulling a site from pantheon
- Visit sites dashboard, dev environment tab, 'Connection Info' button, copy Git url
````
cd /cgywin/c/webroot
git clone ssh://codeserver.dev.{{siteID}}@codeserver.dev.{{siteID}}.drush.in:2222/~/repository.git destination-folder-name
cd destination-folder-name
````
- Run Composer to get all dependencies
````
composer update
````
- Start Docker and create folders
````
docker-compose up -d
docker-composer exec php sh
mkdir ~/.drush
mkdir ~/.ssh
exit
````
- Clone alias file and ssh key into container
````
cd ~/.drush
docker cp alias-file-name.php container_name_php:/root/.drush
cd ~/.ssh
docker cp id_rsa container_name_php:/root/.ssh
````
- Update permissions for ssh key
````
cd /cgywin/c/webroot/destination-folder-name
docker-compose exec php sh
chmod 400 ~/.ssh/id_rsa
exit
````
- Pull database from Pantheon
````
ddrush sql-sync @live @local
````
- Get sftp details from Live environment Connection Info and download drupal files using ftp program