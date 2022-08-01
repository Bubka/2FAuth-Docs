---
order: 100
---
# Self-hosted server

You can deploy 2FAuth on your own web server, whether on your local computer or a web host.
The following guide describes how to proceed and gives basic configurations for both NGINX and Apache2 web servers.  

## Requirements

### HTTP server

__Apache__ and __NGINX__ are the most popular web servers. If you rent a server or web hosting, you probably already have one of them installed. If you plan to use your own machine and need help installing and configuring a web server, please consider searching the Web, as there are many tutorials to guide you through.

- <a href="https://www.google.com/search?q=install+apache2" target="_blank">Google search for `install apache2`</a>
- <a href="https://www.google.com/search?q=install+nginx" target="_blank">Google search for `install nginx`</a>

### PHP

- PHP >= [!badge 8.0]
- BCMath PHP Extension
- Ctype PHP Extension
- Fileinfo PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension

Depending on the chosen database (see below), don't forget to install the corresponding PHP extension (i.e `php8.0-sqlite3` or `php8.0-mysql`)

### Database

You need a database to run 2FAuth. Supported databases are the ones supported by Laravel.

- MariaDB [!badge 10.2+]
- MySQL [!badge 5.7+]
- PostgreSQL [!badge 9.6+]
- SQLite [!badge 3.8.8+]
- SQL Server [!badge 2017+]

!!! Recommendation
2FAuth is a very light application with minimal needs and no concurrent connections, so __SQLite__ is probably the best choice.
!!!

### Composer

You need __Composer__ to install all PHP dependencies of 2FAuth. As the installation process of Composer may change depending on your operating system, please follow the instructions provided on the official website:

[!ref icon="package-dependents" target="blank" text="Install Composer"](https://getcomposer.org/doc/00-intro.md)

You can test your installation by running `php composer.phar -v` in a terminal (or just `composer -v` if composer has been installed in a directory that is part of your system PATH)

## Server configuration

!!! Installation path
We will use `/var/www/2fauth` which is a common path in the *nix world. Of course, you are free to use another path, just remember to adapt the following configurations and commands with yours.
!!!

+++ NGINX

Set your NGINX configuration in `/etc/nginx/nginx.conf` as :

```nginx
events {}
http {
  include mime.types;

  access_log /dev/stdout;
  error_log /dev/stderr;

  server {
      listen 80;
      server_name 2fAuth;
      root /var/www/2fauth/public;

      index index.php;

      charset utf-8;

      location / {
          try_files $uri $uri/ /index.php?$query_string;
      }

      location = /favicon.ico { access_log off; log_not_found off; }
      location = /robots.txt  { access_log off; log_not_found off; }

      error_page 404 /index.php;

      location ~ \.php$ {
          fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
          fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
          include fastcgi_params;
      }

      location ~ /\.(?!well-known).* {
          deny all;
      }
  }
}
```

You can verify the Nginx configuration is valid with:

```sh
nginx -t
```

+++ Apache2

Add a new virtual host:

```bash
cd /etc/apache2/sites-available
sudo nano 2fauth.conf
```

Add the following to the newly created file:

```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAdmin webmaster@example.com
    DocumentRoot /var/www/2fauth/public

    <Directory /var/www/2fauth/public>
        Options Indexes MultiViews
        AllowOverride None
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

!!! warning
Remember to replace example.com with your domain
!!!

Save and close the file, then enable the new VHost and restart Apache2:

```bash
# Assuming 000-default.conf is your current enabled v-host
# Disable the current v-host
sudo a2dissite 000-default.conf

# Enable the 2fauth v-host
sudo a2ensite 2fauth

# Restart apache2
sudo systemctl restart apache2
```

+++

## Set up 2FAuth

### Get the source code

As a reminder, the installation path for this guide is `/var/www/2fauth`. The given commands should be modified if you are using another path.

+++ Using cli

=== Get the latest release

```bash
mkdir -p /var/www/2fauth
curl https://api.github.com/repos/Bubka/2FAuth/tags | grep "tarball_url" | \
    grep -Eo 'https://[^\"]*' | sed -n '1p' | xargs wget -O - | tar -xz --strip-components=1 -C /var/www/2fauth
```

=== Get a specific release

```bash
mkdir -p /var/www/2fauth
# list existing 2FAuth versions
curl https://api.github.com/repos/Bubka/2FAuth/releases | grep "\"name\"" | grep -Eo 'v[^\"]*'
# Replace 3.0.0 with the version of your choice
VERSION=v3.0.0
wget -qO- "https://github.com/Bubka/2FAuth/archive/refs/tags/${VERSION}.tar.gz" | \
    tar -xz --strip-components=1 -C /var/www/2fauth
```

===

+++ Using Git

=== Get the latest release

```bash
git clone https://github.com/bubka/2fauth.git /var/www/2fauth
cd /var/www/2fauth
curl https://api.github.com/repos/Bubka/2FAuth/releases/latest | grep "\"name\"" | grep -Eo 'v[^\"]*' | xargs git checkout
```

=== Get a specific release

```bash
git clone https://github.com/bubka/2fauth.git /var/www/2fauth
cd /var/www/2fauth
# list existing 2FAuth versions
git tag
# Replace 3.0.0 with the version of your choice
git checkout v3.0.0
```

===

+++ Download from GitHub

1. Download the source code of the <a href="https://github.com/Bubka/2FAuth/releases/latest" target="_blank">latest 2FAuth release</a>
2. Extract the archive to a `temp` folder
3. Open the top folder named `temp/2fauth-x.y.z`
4. Move its content to `/var/www/2fauth`

+++

### Install composer dependencies

From the `/var/www/2fauth/` directory:

```sh
composer install --prefer-dist --no-scripts --no-dev
```

Or if you didn't add composer to your system PATH:

```sh
php composer.phar install --prefer-dist --no-scripts --no-dev
```

### Create the database

Use the CLI of the chosen database to create a new database with one of the following commands:

+++ SQLite

```sh
sqlite> .open /var/www/2fauth/database/database.sqlite
sqlite> .quit
```

Reference

[!ref icon="book" target="_blank" text="Command Line Shell For SQLite"](https://www.sqlite.org/cli.html#opening_database_files)

+++ MySQL / MariaDB

```mysql
mysql> CREATE DATABASE 2fauth;
```

Reference

[!ref icon="book" target="_blank" text="Creating database with MySQL"](https://dev.mysql.com/doc/refman/5.7/en/creating-database.html)
[!ref icon="book" target="_blank" text="Creating database with MariaDB"](https://mariadb.com/kb/en/create-database)

+++ PostgreSQL

```sql
CREATE DATABASE 2fauth
```

Reference

[!ref icon="book" target="_blank" text="Creating database with MariaDB"](https://www.postgresql.org/docs/current/sql-createdatabase.html)

+++

If you are not comfortable with the command line, you may use a db management tool like _Adminer_ to ease this step.

[!ref icon="package-dependents" target="blank" text="Get Adminer"](https://www.adminer.org/)

### Set the .env file

Run the following command from the `/var/www/2fauth` directory to create a fresh `.env` file from the `.env.example` template:

```sh
mv .env.example .env
```

Open the `.env` file with a text editor, you will find all environment variables that could be customized.  
You won't have to set/change all, most of them have a default value that will probably fit your needs. But the database and email parts have to be reviewed.

#### Database

+++ When using SQLite

Set the path to your SQLite database file:

```env
DB_DATABASE="/var/www/2fauth/database/database.sqlite"
```

+++ When using MySQL / MariaDB

Uncomment the dedicated lines (and comment the SQLite ones) and replace values with yours:

```env
DB_CONNECTION=mysql
DB_HOST=ip.of.your.server
DB_PORT=3306
DB_DATABASE=2fauth
DB_USERNAME=sqlUserName
DB_PASSWORD=sqlUserPassword
```

+++ When using PostgreSQL

Uncomment the dedicated lines (and comment the SQLite ones) and replace values with yours:

```env
DB_CONNECTION=pgsql
DB_HOST=ip.of.your.server
DB_PORT=5432
DB_DATABASE=2fauth
DB_USERNAME=sqlUserName
DB_PASSWORD=sqlUserPassword
```

+++

#### Email

Email configuration depends on your email provider. You should refer to its documentation to find the relevant values.  
As an example, here is the configuration for an OVH hosting:

```env
MAIL_DRIVER=smtp
MAIL_HOST=SSL0.OVH.NET
MAIL_PORT=465
MAIL_USERNAME=john.doe@example.com
MAIL_PASSWORD=MyP4Ssw0rd
MAIL_ENCRYPTION=ssl
MAIL_FROM_NAME="John"
MAIL_FROM_ADDRESS=john.doe@example.com
```

### Run Artisan commands

Run the following `Artisan` commands from the `/var/www/2fauth/` directory to set up the Laravel part:

```bash
php artisan migrate:refresh
php artisan passport:install
php artisan storage:link
php artisan config:cache
```

## Set ownership and permissions

From the `/var/www/` directory :

```sh
sudo chown -R www-data:www-data 2fauth
sudo chmod -R 775 2fauth
```
