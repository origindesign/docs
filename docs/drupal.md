# Drupal

## Setting up a new site from codebase

See [Origin Drop 8 project](https://github.com/origindesign/origin-drop-8) and follow read me instructions to setup a new Drupal site

## Drupal updates

- Run composer to pull updates locally, update database and export config
````
dcomposer update
ddrush @local updb
ddrush @local cex (just in case any db changes)
````
- Push through circle build then update db and import changes on test/live
````
ddrush @live updb
ddrush @live cim 
````

