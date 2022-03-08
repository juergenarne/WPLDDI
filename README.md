# WordPress Docker image for local development

@author: [Juergen Arne Klein](http://juergen-arne-klein.de)

Adapted from https://registry.hub.docker.com/_/wordpress

Before you begin: 

1. add `127.10.10.10 wordpress.dck`  to your /etc/hosts file
2. add IP `127.10.10.10` to your local network: `sudo ifconfig lo0 alias 127.10.10.10 `
   (https://stackoverflow.com/questions/61688386/listen-tcp-127-0-4-12040-bind-cant-assign-requested-address)
3. run `docker-compose up -d` in this directory

Here is how the `docker-compose.y` file is supposed to look like: 

````yaml
version: '3.9'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - "127.10.10.10:80:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress_user
      WORDPRESS_DB_PASSWORD: wordpress_p4ssw0rd
      WORDPRESS_DB_NAME: wordpress_db
    volumes:
      - wordpress:/var/www/html
      - ./wp/themes:/var/www/html/wp-content/themes
      - ./wp/plugins:/var/www/html/wp-content/plugins
      - ./wp/config/wp-config-local.php:/var/www/html/wp-config.php
      - ./wp/config/.htaccess:/var/www/html/.htaccess

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: wordpress_user
      MYSQL_PASSWORD: wordpress_p4ssw0rd
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - ./mysql/data:/var/lib/mysql
      - ./mysql/logs:/var/log/mysql
      - ./mysql/conf:/etc/mysql

volumes:
  wordpress:
  
````

Please do not modify the files `./wp/config/.htaccess`  and `./wp/config/wp-config-local.php` .

To login to WordPress use `user: admin`  and `pass: admin` 

The directories `./wp/plugins`  and `./wp/themes`  hold the plugins and themes for development reasons.

Happy coding

