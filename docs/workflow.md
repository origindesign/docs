# Workflow

## Overview
Our sites are generally hosted with Pantheon using an external GitHub repository. When changes are pushed to the repo, Circle CI is used to run a series of testing steps before pushing an artifact to the Pantheon repository.

## Setting up a new site from codebase

See [Origin Drop 9 project](https://github.com/origindesign/origin-drop-9) and follow read me instructions to setup a new Drupal site

## Pulling a site to local
- Login to GitHub, go to the repository page, click 'Clone or Download' and copy the git URL
````
cd c:/webroot
git clone git@github.com:origindesign/<repo-name>.git
cd <repo-name>
````
- Start the project using Lando, which will run a composer install
````
lando start
````
- Copy the contents from [development.services.txt](https://github.com/origindesign/origin-drop-9/blob/master/web/sites/development.services.txt) and save as /web/sites/development.services.yml
- Copy the contents from [settings.local.php.txt](https://github.com/origindesign/origin-drop-9/blob/master/web/sites/default/settings.local.php.txt) and save as /web/sites/default/settings.local.php
- Pull database from Pantheon
````
lando pull
````
- Get sftp details from Live environment Connection Info and download drupal files folder using ftp program

- Install dev pre-processing dependencies from theme
````
cd web/themes/custom/origin
npm install
npm start
````
- Clear cache to ensure a fresh start
````
lando drush cr
````
- Visit site in browser
````
https://<site-name>.lndo.site
````

## Making local changes, testing and creating a Pull Request
- From the project root, pull any file changes from git, install any new dependencies with composer and sync the database with live.
````
git pull
composer install
lando pull
````
- Create a branch for your working code
````
git checkout -b <branch-name>
````
- Make all your local changes to files, if you have database changes, run a config export
````
lando drush cex
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
- Once all code changes are tested, create a new Pull Request
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
 composer install
 lando drush cim
````
- Visit Circle CI and watch for PR build to pass
- Login to Pantheon dashboard and push changes from Dev to Test. If you have config or database updates, use terminus to run drush commands
````
lando terminus remote:drush <site-name>.test -- updb
lando terminus remote:drush <site-name>.test -- cim
````
- Test your changes to ensure everything is as expected
- Push changes from Test to Live. If you have config or database updates, run drush commands
````
lando terminus remote:drush <site-name>.live -- updb
lando terminus remote:drush <site-name>.live -- cim
````
- Test your changes to ensure everything is as expected
