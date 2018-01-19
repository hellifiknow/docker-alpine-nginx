# blunt1337/nginx-php7

[![](https://images.microbadger.com/badges/image/blunt1337/nginx-php7.svg)](https://microbadger.com/images/blunt1337/nginx-php7)

This is a [Docker image](https://www.docker.com/) to use as a web server with Nginx and PHP 7.1.
Nginx and PHP are both preconfigured and ready to handle your connections.

PHP includes the following modules:
* json
* gd
* curl
* opcache
* pdo_mysql
* mbstring
* session
* openssl

## Simple usage
Just start it with `docker run -d -p 80:80 -v .:/app blunt1337/nginx-php7 ` to use the current path as webroot.

## Custom usage
The following configuation can be changed with [build args](https://docs.docker.com/compose/compose-file/#args).
You can check the tests folder of this github for samples.

* APP_DIR
	Application file directory, default to /app

* STATIC_DIR
	Document root in APP_DIR.
	With STATIC_DIR=public, the url http://host/ will fetch /app/public/index.php.
	Default to APP_DIR root.

* USER
	User and group running nginx/php process

* HTTPS
	Possible values are:
	* off: Only listen for http.
	* on: Listen for http and https.
	* force: Listen for http and https, and redirect http to https.
	Certificates must be placed as `/etc/nginx/ssl/fullchain.pem` and  `/etc/nginx/ssl/privkey.pem`.

* DOMAINS
	List of allowed domains. Space separated. Default to all.

* RAM
	Server RAM in MB to calculate worker numbers, etc. Be default it will take the builder's machine max RAM.

* UPLOAD_MAX
	Maximum upload file size in MB, default to 10MB

## I need more PHP modules
To install additionnal modules, let's say sqlite, go to [alpine packages](https://pkgs.alpinelinux.org/packages) and search for php7*sqlite. You will find `php7-pdo_sqlite`. Just add the line `CMD apk add --update php7-pdo_sqlite@testing` inside your Dockerfile to install it.

## I want to change nginx configuration
All nginx server configuration blocks are stored in `/etc/nginx/servers/*.conf`. If you want to edit the default configuration, or add another one, create a file in your project like this one:
```Nginx
server {
	listen 80 default_server;
	root "/app";
	
	...
}
```
then add in your Dockerfile `COPY my_custom.conf /etc/nginx/servers/default.conf`.

## Extend this image
Use the onbuild version that runs the setup files in the extended image.
You can check the sample project [nginx-php7-laravel](https://github.com/blunt1337/docker-alpine-nginx-php7-laravel) as a sample.