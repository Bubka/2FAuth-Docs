---
order: 100
---
# Self-hosted server

You can deploy 2FAuth on your own web server, virtual or not, whether on your local computer or a web host.

## Requirements

### PHP

As 2FAuth is develop on top of the Laravel framework, the requirements are <a href="https://laravel.com/docs/deployment#server-requirements" target="_blank">those of Laravel</a>:

- PHP >= [!badge 7.3]
- BCMath PHP Extension
- Ctype PHP Extension
- Fileinfo PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension

Depending on the choosen database (see below), don't forget to install the correspondent PHP extension (i.e `php7.3-sqlite3` or `php7.3-mysql` )

### Database

Like PHP extensions, supported databases are those of Laravel:

- MariaDB [!badge 10.2+]
- MySQL [!badge 5.7+]
- PostgreSQL [!badge 9.6+]
- SQLite [!badge 3.8.8+]
- SQL Server [!badge 2017+]

!!! Recommendation
2FAuth is a very light application with minimal needs and no concurrent connexion so __SQLite__ is probably the best choice.
!!!

### Web server

__Apache__ and __NGINX__ are the most popular web servers. The purpose of this documentation is not to teach you how to configure a web server, but as a starting point you may use one of these configuration files and customize it to fit your running environment.

==- NGINX configuration file

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name 2fauth;
    root /srv/public;
 
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
 
    index index.php;
 
    charset utf-8;
 
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
 
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
 
    error_page 404 /index.php;
 
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

==- APACHE configuration file

```apache
NameVirtualHost *:80
Listen 80
 
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/html/2fauth/public
     
    <Directory /var/www/html/2fauth/public/>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
            Order allow,deny
            allow from all
            Require all granted
    </Directory>
     
    LogLevel debug
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

==-

In addition please consider using the following resources to complete your web server configuration:

- <a href="https://www.google.com/search?q=setup+laravel+apache2" target="_blank">Google search for `setup laravel apache2`</a>
- <a href="https://www.google.com/search?q=setup+laravel+nginx" target="_blank">Google search for `setup laravel nginx`</a>

### Composer

You need __Composer__ to install all PHP dependencies of 2FAuth. As the installation process of Composer may change, please follow the instructions provided on the offical website:

[!ref icon="package-dependents" target="blank" text="Install Composer"](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos)

You can test your installation by running `php composer.phar -v` in a terminal (or just `composer -v` if composer has been install in a directory that is part of your PATH)

## Install 2FAuth

### Get the source code

First identify the latest version of __2FAuth__ from the <a href="https://github.com/Bubka/2FAuth/releases" target="_blank">GitHub release page</a>. The current version is `2.1.0`.

Then, using the method of your choice, download the corresponding source code to the directory declared in your web server configuration. Probably `/var/www/2fauth` for Apache and `/srv` for NGINX.

+++ Using wget

```bash For NGINX
cd /srv
VERSION=v2.1.0
wget -qO- "https://github.com/Bubka/2FAuth/archive/refs/tags/${VERSION}.tar.gz" | \
    tar -xz --strip-components=1 -C /srv
```

or

```bash For Apache
cd /var/www/2fauth
VERSION=v2.1.0
wget -qO- "https://github.com/Bubka/2FAuth/archive/refs/tags/${VERSION}.tar.gz" | \
    tar -xz --strip-components=1 -C /var/www/2fauth
```

+++ Using git

```bash For NGINX
git clone https://github.com/bubka/2fauth.git /srv
cd /srv
git checkout v2.1.0 # replace 2.1.0 with the last version number
```

or

```bash For Apache
git clone https://github.com/bubka/2fauth.git /var/www/2fauth
cd /var/www/2fauth
git checkout v2.1.0 # replace 2.1.0 with the last version number
```

+++ Download from GitHub

1. Download the source code of the <a href="https://github.com/Bubka/2FAuth/releases" target="_blank">latest 2FAuth release</a>
2. Extract the archive to a `temp` folder
3. Open the first subfolder named `temp/2fauth-2.1.0` (replace `2.1.0` with the last version number)
4. Open the web server directory (`/var/www/2fauth` for Apache and `/srv` for NGINX)
5. Move the content of the `temp/2fauth-2.1.0` folder to the web server directory

+++



### Create your database

Each database system has its own command to create a new database so please refer to your database documentation to proceed.

- <a href="https://dev.mysql.com/doc/refman/5.7/en/creating-database.html" target="_blank">Creating database with MySQL</a>
- <a href="https://mariadb.com/kb/en/create-database/" target="_blank">Creating database with MariaDB</a>
- <a href="https://www.sqlite.org/quickstart.html" target="_blank">Creating database with SQLite</a>

If you are not comfortable with the command line you may use a db management tool like _Adminer_ to ease this step.

[!ref icon="package-dependents" target="blank" text="Get Adminer"](https://www.adminer.org/)


