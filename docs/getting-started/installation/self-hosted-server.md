---
order: 100
---
# Self-hosted server

You can deploy 2FAuth on your own web server, virtual or not, whether on your local computer or a web host.

## Requirements

### HTTP server

__Apache__ and __NGINX__ are the most popular web servers. If you rent a server or a web hosting you probably already have one of them installed. If you plan to use your own machine and need help to install and configure it please consider searching the Web, there is many tutorials to guide your through.

- <a href="https://www.google.com/search?q=install+apache2" target="_blank">Google search for `install apache2`</a>
- <a href="https://www.google.com/search?q=install+nginx" target="_blank">Google search for `install nginx`</a>

For simplicity along the following sections

### PHP

As 2FAuth is develop on top of the Laravel framework, the requirements are <a href="https://laravel.com/docs/8.x/deployment#server-requirements" target="_blank">those of Laravel</a>:

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

Depending on the choosen database (see below), don't forget to install the corresponding PHP extension (i.e `php7.3-sqlite3` or `php7.3-mysql` )

### Database

You need a database to run 2FAuth. Like PHP extensions, supported databases are the ones supported by Laravel. The choice is yours:

- MariaDB [!badge 10.2+]
- MySQL [!badge 5.7+]
- PostgreSQL [!badge 9.6+]
- SQLite [!badge 3.8.8+]
- SQL Server [!badge 2017+]

!!! Recommendation
2FAuth is a very light application with minimal needs and no concurrent connection so __SQLite__ is probably the best choice.
!!!

### Composer

You need __Composer__ to install all PHP dependencies of 2FAuth. As the installation process of Composer may change, please follow the instructions provided on the offical website:

[!ref icon="package-dependents" target="blank" text="Install Composer"](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos)

You can test your installation by running `php composer.phar -v` in a terminal (or just `composer -v` if composer has been install in a directory that is part of your system PATH)

## Install 2FAuth

The following sections describe how to install 2FAuth

### Get the source code

Using the method of your choice, download the source code of the <a href="https://github.com/Bubka/2FAuth/releases" target="_blank">latest 2FAuth release</a> to the directory watched by your web server.

+++ Using cli

```bash Get the latest release
mkdir -p /var/www/2fauth
curl https://api.github.com/repos/Bubka/2FAuth/tags | grep "tarball_url" | \
    grep -Eo 'https://[^\"]*' | sed -n '1p' | xargs wget -O - | tar -xz --strip-components=1 -C /var/www/2fauth
```

```bash Get a specific release
mkdir -p /var/www/2fauth
VERSION=v2.1.0 # Replace with the version of your choice
wget -qO- "https://github.com/Bubka/2FAuth/archive/refs/tags/${VERSION}.tar.gz" | \
    tar -xz --strip-components=1 -C /var/www/2fauth
```

+++ Using Git cli

```bash Get the latest release
git clone https://github.com/bubka/2fauth.git /var/www/2fauth
cd /var/www/2fauth
curl https://api.github.com/repos/Bubka/2FAuth/releases/latest | grep "\"name\"" | grep -Eo 'v[^\"]*' | git checkout
```

```bash Get a specific release
git clone https://github.com/bubka/2fauth.git /var/www/2fauth
cd /var/www/2fauth
git checkout v2.1.0 # Replace 2.1.0 with the version of your choice
```

+++ Download from GitHub

1. Download the source code of the <a href="https://github.com/Bubka/2FAuth/releases/latest" target="_blank">latest 2FAuth release</a>
2. Extract the archive to a `temp` folder
3. Open the top folder named `temp/2fauth-x.y.z`
4. Move its content to `/var/www/2fauth`

+++

### Create your database

Each database system has its own command to create a new database so please refer to your database documentation to proceed.

- <a href="https://dev.mysql.com/doc/refman/5.7/en/creating-database.html" target="_blank">Creating database with MySQL</a>
- <a href="https://mariadb.com/kb/en/create-database/" target="_blank">Creating database with MariaDB</a>
- <a href="https://www.sqlite.org/quickstart.html" target="_blank">Creating database with SQLite</a>

If you are not comfortable with the command line you may use a db management tool like _Adminer_ to ease this step.

[!ref icon="package-dependents" target="blank" text="Get Adminer"](https://www.adminer.org/)
