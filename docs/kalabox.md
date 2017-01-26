# Kalabox Setup

## How to make Kalabox reading web directory

- Pull the Drupal site you want to run on Kalabox from Pantheon
- Run the Site and in bash, type:
- In 
```bash
$ docker ps --all | grep myappname
```
- Get the docker name of your nginx container and ssh into it:
```bash
$ docker exec -i -t nameofyourcontainer bash
```
- Then install vim to edit the conf file of nginx
```bash
$ apt-get update
$ apt-get install vim
$ vim /etc/nginx/conf.d/default.conf
```
- Change the root to root /code to root /code/web
- Change the 2 instances of "fastcgi_param SCRIPT_FILENAME /code/$fastcgi_script_name;" to "fastcgi_param SCRIPT_FILENAME /code/web/$fastcgi_script_name;".
- Exit the container
- Restart Kalabox

