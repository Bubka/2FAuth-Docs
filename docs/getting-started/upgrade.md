---
order: -100
---
# Upgrade

## Self-hosted server

Update the source code in `/var/www/2fauth` (see [Get the source code](/getting-started/installation/self-hosted-server/#get-the-source-code))

!!!warning
Do not change the `/var/www/2fauth/storage` directory nor your `/var/www/2fauth/database/database.sqlite` file (when using SQLite)
!!!

Then run the following commands:

```sh
php composer.phar update
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
