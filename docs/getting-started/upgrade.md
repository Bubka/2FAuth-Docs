---
order: -1000
---
# Upgrade

## Self-hosted server

Update the source code in `/var/www/2fauth` (see [Get your 2FAuth copy](/getting-started/installation/self-hosted-server/#get-your-2fauth-copy))

!!!warning
Do not change the `/var/www/2fauth/storage` directory nor your `/var/www/2fauth/database/database.sqlite` file (when using SQLite)
!!!

Depending on how you update your files, you could have to set the permissions again:

```sh
chown -R www-data:www-data /var/www/2fauth
chmod -R 775 /var/www/2fauth
```

Update the dependencies by running:

```sh
composer install

# or if you didn't add composer to your system PATH
php composer.phar install
```

Then start the installation wizard by running:

```sh
php artisan 2fauth:install
```

The wizard automatically runs the following commands:

```sh
php artisan cache:clear
php artisan config:clear
php artisan migrate
php artisan passport:install
php artisan config:cache
php artisan route:cache
```

## Docker

!!!warning When using SQLite
At the very least, backup your `database.sqlite` file to avoid bad surprises!
!!!

The Docker image [!badge 2fauth/2fauth] is built on every commit pushed to the `master` branch.  
You can therefore pull the image with `docker pull 2fauth/2fauth` and restart the container to update it.

You can also use tagged images, see [Docker Hub tags](https://hub.docker.com/r/2fauth/2fauth/tags?page=1&ordering=last_updated), which are produced on Github releases.

## YunoHost

1. Open the __:icon-sync: System update__ manager from the Yunohost Admin
2. In the __:icon-package: Applications__ section, click the 2FAuth [!button variant="success" icon="plus" corners="round" text="Upgrade" size="s"] button
3. Wait for the installer to complete its job

You can also upgrade using the YunoHost command-line:

```bash
yunohost app upgrade 2fauth
```
