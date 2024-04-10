# Troubleshooting

## Check logs

Logs can give usefull informations to troubleshoot your installation. 2FAuth logs are stored in the subfolder `storage/logs` of your installation folder.

You may enable debug logs by setting `APP_DEBUG=true` and `LOG_LEVEL=debug` in your .env file.

The web server and the database server also provide some logs. Their locations may vary depending on the server choice and your operating system. If you followed the [Web server configuration](#web-server-configuration) of this guide, the web server logs should be in one of these locations under a *nix system :

+++ NGINX

```bash
/dev/stdout
/dev/stderr
```

+++ Apache2

```bash
# Debian / Ubuntu 
/var/log/apache2/

# RHEL / Red Hat / CentOS / Fedora 
/var/log/httpd/

# FreeBSD
var/log/
```

+++

## Possible issues

__The uploaded icons are not visible even though I set the storage symlink__
:   Try to recreate the symlink using relative path.

    Open a terminal on the 2FAuth installation folder and run:

    ```bash
    ln -sfn ../storage/app/public public/storage
    ```

__2FAuth returns a `500` error with ionos hosting__
:   The `.htaccess` configuration should be modified.

    Edit the `/public/.htaccess` file and add following lines:

    ```apache
    RewriteBase /
    Options +FollowSymLinks
    ```

    right before:

    ```apache
    RewriteEngine On
    ```

__2FAuth returns a `404` error on API requests__
:   If using Apache2, ensure permissions are set correctly.

    Open a terminal and run:

    ```bash
    sudo chown -R www-data:www-data /var/www/2fauth
    sudo chmod -R 775 /var/www/2fauth
    ```
    
    Also, ensure `mod_rewrite` is enabled:

    ```bash
    sudo a2enmod rewrite
    systemctl restart apache2
    ```

__Firefox warns of insecure connection when behind a proxy__
:   Your proxy should be registered via an enviroment variable.

    Edit your .env file and set:

    ```env
    TRUSTED_PROXIES=your_proxy_ip_address
    ```
    In case of multiple proxies, separate the addresses with a comma.
