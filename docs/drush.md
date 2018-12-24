# Drush

## List of useful drush commands

- Enable/update a module
````
drush @alias up module_name
````

- Clear all caches
````
drush @alias cr
````

- Sync a database
````
drush sql-sync @from-alias @to-alias
````

- Config export
````
drush @alias cex
````

- Config import
````
drush @alias cim 
````