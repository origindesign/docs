# Terminus
Terminus can be used to run commands directly on Pantheon sites. It is useful for running drush commands instead of using alias files
- List of commands [https://pantheon.io/docs/terminus/commands/](https://pantheon.io/docs/terminus/commands/) 
````
terminus remote:drush site-name.live -- updb
terminus remote:drush site-name.live -- cim
````