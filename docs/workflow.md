# Workflow
- Instead of running drush commands through alias files, Terminus can be used with remote:drush [See here](https://origindocs.readthedocs.io/en/latest/terminus)
- Ensure shortcuts are added to ~/.bashrc [See Here](https://origindocs.readthedocs.io/en/latest/docker/) 
- Edit ~/.gitconfig
````
[user]
	name = <Firstname Lastname>
	email = <email@domain.com>
[core]
	autocrlf = true
````

## Pulling a site to local
- Login to GitHub, go to the repository page, click 'Clone or Download' and copy the git URL
````
cd c:/webroot
git clone git@github.com:origindesign/<repo-name>.git
cd <project-folder-name>
````
- From the project root, get all dependencies with Composer
````
composer install
````
- To avoid potential drush sql-sync issues, delete drush, then run composer install from inside the container to re-install
````
rm -rf vendor/drush
rm -rf vendor/bin/drush*

dcomposer install
-- or
docker-composer exec php sh
composer install
exit
````

- Start Docker and create folders within docker container
````
docker-compose up -d
docker-compose exec php sh
mkdir ~/.drush
mkdir ~/.ssh
exit
````
- Clone alias file and ssh key into container
````
cd <location-of-alias-file>
docker cp <alias-file-name> <container_name>_php:/root/.drush
cd ~/.ssh
docker cp id_rsa <container_name>_php:/root/.ssh
````
- Update permissions for ssh key
````
cd c:/webroot/<project-folder-name>
docker-compose exec php sh
chmod 400 ~/.ssh/id_rsa
exit
````
- Copy the contents from [https://github.com/origindesign/origin-drop-8/blob/master/web/sites/default/settings.local.php.txt](https://github.com/origindesign/origin-drop-8/blob/master/web/sites/default/settings.local.php.txt) and save as /web/sites/default/settings.local.php
- Pull database from Pantheon
````
ddrush sql-sync @live @local
````
- Get sftp details from Live environment Connection Info and download drupal files folder using ftp program

- Add URL alias to windows hosts file so you can access from a web browser
- C:\Windows\System32\drivers\etc\hosts
````
127.0.0.1    <site-name>.docker.localhost
127.0.0.1    pma.<site-name>.docker.localhost
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
http://<site-name>.docker.localhost:8000
````

## Making local changes, testing and creating a Pull Request
- From the project root, pull any file changes from git, install any new dependencies with composer and sync the database with live. If there are any drush related issues syncing the live database, within the Pantheon dashboard clone it to dev or test and sync from there
````
git pull
dcomposer install
ddrush sql-sync @live @local
````
- Create a branch for your working code
````
git checkout -b <branch-name>
````
- Make all your local changes to files, if you have database changes, run a config export
````
ddrush @local cex
````
- Commit and push your changes noting the FreshDesk ticket number if one exists and a short descriptive comment
````
git add -A .
git commit -m "FD **** - Brief description of changes made"
git push origin <branch-name>
````
- Visit [https://circleci.com](https://circleci.com) and login with GitHub
- Visit https://circleci.com/gh/origindesign/ <git-repo-name> and watch for build to pass
- Login to Pantheon dashboard, visit the sites Multidev tab and click on <branch-name>
- Click on 'Visit <branch-name>' to view your site and complete all relevant testing
- Once all code changes are tested create a new Pull Request
  - Visit https://github.com/origindesign/ <git-repo-name>
  - Click 'Branches' tab and next to <branch-name> click 'New pull request'
  - Click the settings icon besides 'Assignees' and assign 'origindesign'
  - Enter a comment and press 'Create pull request'

## Reviewing and approving Pull Request by senior developer
- Visit https://github.com/origindesign/ <git-repo-name> to review all code changes within the Pull Request
- Review Multidev site to confirm all changes are functioning as expected
- Once approved, visit Pull Request on GitHub and press 'Merge pull request'
- Update local checkout with changes

 ````
 git pull
 dcomposer install
 ddrush @local cim
 ````
 
- Visit Circle CI and watch for PR build to pass
- Login to Pantheon dashboard and pull changes from Dev to Test. If you have config updates or database updates, run drush commands
````
ddrush @test updb && ddrush @test cim
````
- Test your changes to ensure everything is as expected
- Pull changes from Test to Live. If you have config updates or database updates, run drush commands
````
ddrush @live updb && ddrush @live cim
````
- If you imported config, ensure AdVagg is switched back on
- Configuration > Development > Performance > AdvVagg >
Check 'Enable advanced aggregation' or simply Clear Cache from within the Pantheon Dashboard and this will fire a Quicksilver script to turn this back on with 'drush cset advagg.settings enabled 1 -y'
- Test your changes to ensure everything is as expected
