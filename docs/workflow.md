# Workflow

## Pulling a site to local
- Login to GitHub, go to the repository page, click 'Clone or Download' and copy the git URL
````
cd /cgywin/c/webroot
git clone git@github.com:origindesign/repo-name.git destination-folder-name
cd destination-folder-name
````
- From the project root, get all dependencies with Composer
````
composer install
````
- Start Docker and create folders
````
docker-compose up -d
docker-compose exec php sh
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
- Copy the contents from below and save as /web/sites/default/settings.local.php
````
https://github.com/origindesign/origin-drop-8/blob/master/web/sites/default/settings.local.php.txt
````
- Pull database from Pantheon
````
ddrush sql-sync @live @local
````
- Get sftp details from Live environment Connection Info and download drupal files using ftp program

- Add URL alias to hosts file so you can access from a web browser
````
127.0.0.1    site-name.docker.localhost
127.0.0.1    pma.site-name.docker.localhost
````

- Install dev pre-processing dependencies from theme
````
cd web/themes/custom/origin
yarn install
gulp watch
````
- Clear cache to ensure a fresh start
````
ddrush @local cr
````
- Visit site in browser
````
http://site-name.docker.localhost
````

## Making local changes, testing and pushing live
- From the project root, pull file changes from git, update composer and import any potential config updates
````
git pull
composer update
ddrush cim
````
- Create a branch for your working code
````
git checkout -b branch-name
````
- Make all your local changes to files, if you have database changes, run a config export
````
ddrush cex
````
- Commit and push your changes
````
git add -A .
git commit -m "Enter FreshDesk ticket number (FD ****) if one exists and briefly describe your changes"
git push origin branch-name
````
- Login to Circle CI and watch for build to pass
- Login to Pantheon dashboard, visit you sites Multidev tab, click on branch-name
- Click on 'Visit branch-name' to view your site and test your code
- Once all code changes are tested and approved, from your local, merge the code into master branch
````
git checkout master
git merge branch-name
git push origin master
````
- Login to Circle CI and watch for build to pass
- Login to Pantheon dashboard and pull changes from Dev to Test. If you have config updates or database updates, run drush commands
````
ddrush @test updb
ddrush @test cim
````
- Test your changes to ensure everything is as expected
- Pull changes from Test to Live. If you have config updates or database updates, run drush commands
````
ddrush @live updb
ddrush @live cim
````
- If you imported config, ensure AdvVagg is switched back on
- Configuration > Development > Performance > AdvVagg >
Check 'Enable advanced aggregation'
- Test your changes to ensure everything is as expected
