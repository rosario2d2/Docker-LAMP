# Docker LAMP Environment

This is a LAMP environment build using Docker and Docker Compose. It has all I need for developing on a local machine:

* Apache `2.4.37`
* PHP-FPM `7.2.11`
* MySQL `5.6.34`
* phpMyAdmin `latest`
* WP-CLI `latest`
* MailHog `latest`

Apache will run on port `80`, phpMyAdmin on port `8080` and MailHog on port `8025`.

## Features

* Basic configuration with environment variables
* Custom `apache2.conf`, `vhosts.conf`, `mysql.cnf`, `phpmyadmin.config.php` and `php.ini` configuration files on local machine
* Apache, MySQL and PHP-FPM logs are stored on the local machine for quick inspection
* MySQL data files are stored on the local machine for persistence
* WP-CLI will run as the `www-data` user so that permissions remain correct
* MySQL user has all privileges on databases named with the `db_` prefix
* Custom colorized prompt for the Apache, MySQL and PHP-FPM images
* MailHog installed and configured

## Configuration

Modify the `.env-sample` file according to preferences and then rename it `.env`.

Using environment variables contained in the `.env` file you can set:

* Timezone and locales for the Apache, PHP-FPM and MySQL images
* A project name that will be used by Docker Compose for the build images and the containers
* MySQL root password and the privileged user username and password

## Installing

Clone this repository on your local machine, configure (see above) and run `docker-compose up`.

```
git clone https://github.com/rosario2d2/Docker-LAMP.git
cd Docker-LAMP
docker-compose up
```
