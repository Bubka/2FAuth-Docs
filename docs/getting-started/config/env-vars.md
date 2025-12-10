---

---
# Environment variables

## Purpose

Here are the main environment variables that may be set to make 2FAuth work according to its running environment and to meet your needs. It is recommended to review them at least once to ensure minimal requirements are met and to have a picture of what 2FAuth offers in terms of configuration.

Asterisks next to the var names aim to identify important vars:

- A blue <span class="expected">asterisk</span> indicates that the var should be set with a suitable value, otherwise 2FAuth may not work as expected.
- A red <span class="mandatory">asterisk</span> indicates that the var must be set, otherwise 2FAuth will not work at all.

!!!primary
Set only the vars that need to be ajusted. If a var is not set, its default value will be applied.
!!!

---

## How To

You can set environment variables in various ways, depending on the running environment you chose.

- When 2FAuth is deployed directly on a server (e.g. a VM or a bare metal server), the most straightforward method is to edit the `.env` file you should have set up during the [installation process](/getting-started/installation/self-hosted-server.md#set-the-env-file).

  !!!secondary
  The `.env.example` file sets most of these variables for the sole purpose of guiding the user during the configuration of 2FAuth.
  !!!
- When running 2FAuth from a Docker container, you can use the `-e "[variable_name]=[new_value]"` or the `--env-file [path_to_env_file]` command-line arguments with the `docker run` command, or set the variables in a _docker-compose_ file.

    [!ref icon="book" target="_blank" text="Ways to set environment variables with Compose"](https://docs.docker.com/compose/environment-variables/set-environment-variables/)

    [!ref icon="book" target="_blank" text="Set environment variables with docker run"](https://docs.docker.com/engine/reference/commandline/run/#env)

!!! Important
The configuration may have been cached. If so, clear the cache before editing the environment variable:

- Run `php artisan config:clear` OR
- Delete the file `[2FAuth_directory]/bootstrap/cache/config.php`

Once variables have been modified, (re)build the cache by running `php artisan config:cache`
!!!

---

## General setting

:::has-following-h3-more-spaced
:::

### APP_NAME

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   The name of the application. It is used when the app name needs to place in a notification or any other location as required by the application or its packages.

Default value
:   `2FAuth`

:::

### APP_ENV

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Determines the "environment" 2FAuth is currently running in.

    If you change it to production, most Artisan console commands will ask for extra confirmation.

    Do not set it to `testing` unless you run dev tests as some features will be disabled.

Possible values
:   `local`, `testing`, `production`

Default value
:   `local`

:::

### APP_TIMEZONE

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.2"]

:::env-var-dl-wrapper

Description
:   The timezone for your application, which is used to record dates and times to database.

    This global setting can be overridden by users via in-app settings for a personalised dates and times display.

    !!!
    If this setting is changed while the application is already running, existing records in the database won't be updated.
    !!!

Possible values
:   See <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones" target="_blank">List of TZ database time zones</a>
Default value
:   `UTC`

:::

### APP_DEBUG

[!badge variant="info" text="boolean"]

:::env-var-dl-wrapper

Description
:   When 2FAuth is in debug mode, detailed error messages with stack traces will be shown on every error that occurs within 2FAuth.

    If disabled, a simple generic error page is shown.

Default value
:   `false`

:::

### <span class="mandatory">APP_KEY</span>

[!badge variant="info" text="string"] [!badge variant="info" text="32 random characters"]

:::env-var-dl-wrapper

Description
:   The encryption key for all security related features (sessions, [DB encryption](/security/data-protection/#db-encryption), [webauthn](/security/authentication/webauthn/), [personal access token](/security/authentication/pat/))

    !!!warning
    Keep this very secure. If you loose it or generate a new one, all existing encrypted data in db must be considered LOST.
    !!!

    You can generate a key with the `php artisan key:generate` command.

Default value
:   _none_

:::

### <span class="expected">APP_URL</span>

[!badge variant="info" text="string"] [!badge variant="info" text="url"]

:::env-var-dl-wrapper

Description
:   The web address (URL) of your 2FAuth instance, e.g. `https://2fauth.mydomain.com`

    !!!primary
    Ensure the value you set uses the `https` scheme when 2FAuth is reached through a secure connection
    !!!

    !!!primary
    If a custom port is used, it must be appended to the URL: `https://2fauth.mydomain.com:8001`
    !!!

    !!!warning
    This __must__ match your instance's external address (the location in your browser address bar) otherwise you'll get a blank page or [WebAuthn](/security/authentication/webauthn/) authentication won't work.
    !!!

Default value
:   `http://localhost`

:::

### ASSET_URL

[!badge variant="info" text="string"] [!badge variant="info" text="url"] [!badge variant="info" text="since v5.0.3"]

:::env-var-dl-wrapper

Description
:   The URL of your 2FAuth assets (CSS & JS files), e.g. `https://2fauth.cdn.com`

    Only set this variable if you want to serve assets from a location other than your primary server, such as a CDN. Otherwise, do not set this variable.

Default value
:   Fallbacks to the [APP_URL](#app_url) value when the var is not set.

:::

### <span class="expected">APP_SUBDIRECTORY</span>

[!badge variant="info" text="string"] [!badge variant="info" text="path"] [!badge variant="info" text="since v4.0"]

:::env-var-dl-wrapper

Description
:   The domain subdirectory from which you want to serve 2FAuth.

    !!!warning
    This must reflect the path targeted by [APP_URL](#app_url).
    !!!

    Example:

    If you previously set `APP_URL` to `https://mydomain.org/2fa` to access 2FAuth from the `/2fa/` subdirectory, you have to set `APP_SUBDIRECTORY=2fa`.

    Leave blank if you serve 2FAuth from the domain root.

Default value
:   _blank_

:::

### IS_DEMO_APP

[!badge variant="info" text="boolean"]

:::env-var-dl-wrapper

Description
:   Makes the 2FAuth instance behave like a demonstration app.

    In Demo mode, the app displays some banners and disables certain features such as the password reset.
    
    !!!primary
    You can feed a demo app with fake data using the artisan command `php artisan 2fauth:reset-demo`.
    
    Setting `IS_DEMO_APP` to `true` is mandatory for this command to run.
    !!!

Default value
:   `false`

:::

## API setting

### THROTTLE_API

[!badge variant="info" text="number"] [!badge variant="info" text="since v4.0"]

:::env-var-dl-wrapper

Description
:   The maximum number of API calls in a minute from the same IP.  
    Once reached, all requests from this IP will be rejected until the minute has elapsed.

    Set to `null` to disable the API throttling.

Default value
:   `60`

:::

## Authentication setting

### LOGIN_THROTTLE

[!badge variant="info" text="number"] [!badge variant="info" text="since v4.0"]

:::env-var-dl-wrapper

Description
:   The number of times per minute a user can fail to log in before being locked out.

    Once reached, all login attempts will be rejected until the minute has elapsed. This setting applies to both email/password and webauthn login attemps.

Default value
:   `5`

:::

### <span class="expected">AUTHENTICATION_GUARD</span>

[!badge variant="info" text="string"] [!badge variant="info" text="since v3.0"]

:::env-var-dl-wrapper

Description
:   The authentication guard used to perform users authentication.

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`web-guard`
:   The default guard to handle web authentications, such as login/password or webauthn.

`reverse-proxy-guard`
:   A guard to handle authentication previously made by an auth proxy (like nginx or authelia) in front of 2FAuth.

    2FAuth only look for the dedicated headers and skip all other built-in authentication checks. That means your proxy is fully responsible of the authentication process, 2FAuth will trust him as long as headers are presents.

    Additional variable is required to be set with this guard:

    - [AUTH_PROXY_HEADER_FOR_USER](#auth_proxy_header_for_user)

    Additional variable may be set with this guard:

    - [AUTH_PROXY_HEADER_FOR_USER](#auth_proxy_header_for_user)

    See the dedicated [auth proxy page](/security/authentication/auth-proxy/) to discover how to configure the reverse-proxy-guard.

:::

:::env-var-dl-wrapper

Default value
:   `web-guard`

Alias
:   `AUTH_GUARD`

:::

### AUTHENTICATION_LOG_RETENTION

[!badge variant="info" text="number"] [!badge variant="info" text="since v5.2"]

:::env-var-dl-wrapper

Description
:   The authentication log retention time, in days.

    Log entries older than that are automatically deleted.

Default value
:   `365`

:::

### AUTH_PROXY_HEADER_FOR_USER

[!badge variant="info" text="string"] [!badge variant="info" text="since v3.0"]

:::env-var-dl-wrapper

Description
:   Name of the HTTP header sent by the authentication proxy. This header identifies the user authenticated at proxy level.

    Check your proxy documentation to find out how this header is named (i.e `REMOTE_USER`)

    Only relevant when [AUTHENTICATION_GUARD](#authentication_guard) is set to `reverse-proxy-guard`.

Default value
:   `REMOTE_USER`

:::

### AUTH_PROXY_HEADER_FOR_EMAIL

[!badge variant="info" text="string"] [!badge variant="info" text="since v3.0"]

:::env-var-dl-wrapper

Description
:   Name of the HTTP header sent by the authentication proxy that provides the email address of the user authenticated at proxy level.

    Check your proxy documentation to find out how this header is named (i.e `REMOTE_EMAIL`)

    Only relevant when [AUTHENTICATION_GUARD](#authentication_guard) is set to `reverse-proxy-guard`.

Default value
:   `null`

:::

### PROXY_LOGOUT_URL

[!badge variant="info" text="string"] [!badge variant="info" text="url"] [!badge variant="info" text="since v3.1"]

:::env-var-dl-wrapper

Description
:   Custom logout URL to open when a user clicks the _Logout_ link in 2FAuth.  
    In most case this would send the user to the logout page of your authentication proxy.

    Only relevant when [AUTHENTICATION_GUARD](#authentication_guard) is set to `reverse-proxy-guard`.

Default value
:   `null`

:::

### WEBAUTHN_NAME

[!badge variant="info" text="string"] [!badge variant="info" text="since v3.0"]

:::env-var-dl-wrapper

Description
:   Name of the Relying Party in the WebAuthn process. This should match the name of the application.

    Do not set to `null`.

Default value
:   Fallbacks to the [APP_NAME](#app_name) value when the var is not set.

:::

### WEBAUTHN_ID

[!badge variant="info" text="string"] [!badge variant="info" text="since v3.0"]

:::env-var-dl-wrapper

Description
:   ID of the Relying Party in the WebAuthn process. This should equal the application domain (i.e 2fauth.example.com).

    While only the [WEBAUTHN_NAME](#webauthn_name) is enough, you can further set a custom domain as ID.  
    If set to `null`, the device will fill it internally.

    See <a href="https://webauthn-doc.spomky-labs.com/prerequisites/the-relying-party#how-to-determine-the-relying-party-id" target="_blank">How to determine the relying party id</a> for more information.

Default value
:   `null`

:::

### WEBAUTHN_USER_VERIFICATION

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Setting to control how user verification behave during the WebAuthn authentication flow.

    See [WebAuthn user verification](/security/authentication/webauthn/#user-verification).

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`required`
:   Will ALWAYS ask for user verification

`preferred`
:   Will ask for user verification IF POSSIBLE

`discouraged`
:   Will NOT ask for user verification (for example, to minimize disruption to the user interaction flow)

:::

:::env-var-dl-wrapper

Default value
:   `preferred`

:::

## Cache setting

### CACHE_DRIVER

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   The default cache store used by 2FAuth when executing caching functions

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`database`
:   Data are cached to the database set with [database setting](#database-setting).

    See <a href="https://laravel.com/docs/cache#prerequisites-database" target="_blank">how to configure the Database driver</a> on the Laravel documentation.

`file`
:   Data are cached to the filesystem

`memcached`
:   A distributed memory object caching system (<a href="https://memcached.org/" target="_blank">memcached.org</a>)

    !!!warning
    This driver requires the installation of an additional PECL package.
    !!!
    
    See <a href="https://laravel.com/docs/cache#memcached" target="_blank">how to configure the Memcached driver</a> on the Laravel documentation.

`redis`
:   An in-memory data structure store, used as a database, cache, and message broker (<a href="https://redis.io/" target="_blank">redis.io</a>)

    !!!warning
    This driver requires the installation of an additional package.
    !!!
    
    See <a href="https://laravel.com/docs/cache#redis" target="_blank">how to configure the Redis driver</a> on the Laravel documentation.

`dynamodb`
:   Serverless, NoSQL, fully managed database with single-digit millisecond performance at any scale (<a href="https://aws.amazon.com/dynamodb/" target="_blank">dynamodb</a>)

    !!!warning
    This driver requires the creation of an additional database table and the installation of the AWS SDK php package.
    !!!
    
    See <a href="https://laravel.com/docs/cache#dynamodb" target="_blank">how to configure the DynamoDB driver</a> on the Laravel documentation.

`array`
:   Convenient cache backend for automated tests

`null`
:   Convenient cache backend for automated tests

:::

:::env-var-dl-wrapper

Default value
:   `file`

Alias
:   `CACHE_STORE`

:::

## Database setting

See [Database configuration](/getting-started/installation/self-hosted-server/#database-1) for information on how to use these variables together.

### <span class="expected">DB_CONNECTION</span>

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   The database driver to be used.

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`mysql`
:   MySQL database

`pgsql`
:   PostgreSQL database

`sqlsrv`
:   SQL Server database

`sqlite`
:   SQLite database

:::

:::env-var-dl-wrapper

Default value
:   `mysql`

:::

### <span class="expected">DB_DATABASE</span>

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Name of the database (when using `mysql`, `pgsql` and `sqlsrv` drivers) or path to the `sqlite` file

    !!!primary
    Using `sqlite` under Windows, path separators have to be escaped:  
    `C:\\path\\to\\database.sqlite`
    !!!

Default value
:   `2fauth`

:::

### <span class="expected">DB_HOST</span>

[!badge variant="info" text="string"] [!badge variant="info" text="ip address"] [!badge variant="info" text="domain"]

:::env-var-dl-wrapper

Description
:   Address of the resource hosting your database.

    Does not apply to `sqlite`.

Default value
:   `127.0.0.1`

:::

### DB_PORT

[!badge variant="info" text="number"]

:::env-var-dl-wrapper

Description
:   Port used to communicate with the database host.

    Does not apply to `sqlite`.

Default values
:   For MySQL: `3306`

    For PostgreSQL: `5432`

    For SQL Server: `1433`

:::

### <span class="expected">DB_USERNAME</span>

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   The username used to connect to the database.

    Does not apply to `sqlite`.

Default value
:   `2fauth`

:::

### <span class="expected">DB_PASSWORD</span>

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   The password used to connect to the database.

    When using `.env` file, if the password contains special characters like `#`, put quotes around it.
    Does not apply to `sqlite`.

Default value
:   An empty string

:::

### DB_CHARSET

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   The character set of the database.

    Does not apply to `sqlite`.

Default values
:   For MySQL: `utf8mb4`

    For PostgreSQL: `utf8`

    For SQL Server: `utf8`

:::

### DB_COLLATION

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   The collation of the database.

    Only applies to `mysql`.

Default value
:   `utf8mb4_unicode_ci`

:::

### DATABASE_URL

[!badge variant="info" text="string"] [!badge variant="info" text="url"]

:::env-var-dl-wrapper

Description
:   A single database "URL" that contains all of the connection information for the database in a single string, i.e:

    `mysql://root:password@127.0.0.1/forge?charset=UTF-8`

    If set, 2FAuth will use it to extract the database connection and credential information. Other `DB_*` vars are then useless.

Default value
:   _none_

Alias
:   `DB_URL`

:::

### MYSQL_ATTR_SSL_CA

[!badge variant="info" text="string"] [!badge variant="info" text="path"]

:::env-var-dl-wrapper

Description
:   Absolute path to the root CA bundle if you're connecting to the MySQL database via SSL

Default value
:   `null`

:::

## Email setting

### <span class="expected">MAIL_MAILER</span>

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   The default mailer that is used to send any email messages sent by 2FAuth.

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`smtp`
:   Use an SMTP server to send emails.

    Additional variables are required to be set with this driver:

    - [MAIL_USERNAME](#mail_username)
    - [MAIL_PASSWORD](#mail_password)

    Additional variables may be set to customize the `smtp` driver configuration:

    - [MAIL_HOST](#mail_host)
    - [MAIL_PORT](#mail_port)
    - [MAIL_ENCRYPTION](#mail_encryption)
    - [MAIL_VERIFY_SSL_PEER](#mail_verify_ssl_peer)

`sendmail`
:   Use a sendmail server to send emails.

    Additional variable may be set to customize the `sendmail` driver configuration:

    - [MAIL_SENDMAIL_PATH](#mail_sendmail_path) 

`mailgun`
:   Use the mailgun API to send emails.

    Additional variables are required to be set with this driver:

    - `MAILGUN_DOMAIN`
    - `MAILGUN_SECRET`
    - `MAILGUN_ENDPOINT`

    See <a href="https://laravel.com/docs/mail#mailgun-driver" target="_blank">how to configure the Mailgun Driver</a> on the Laravel documentation.

`ses`, `ses-v2`
:   Use the Amazon SES API to send emails.

    Additional variables are required to be set with this driver:

    - `AWS_ACCESS_KEY_ID`
    - `AWS_SECRET_ACCESS_KEY`
    - `AWS_DEFAULT_REGION`

    See <a href="https://laravel.com/docs/mail#ses-driver" target="_blank">how to configure the SES Driver</a> on the Laravel documentation.

`postmark`
:   Use the postmark API to send emails.

    Additional variables are required to be set with this driver:

    - `POSTMARK_TOKEN`

    See <a href="https://laravel.com/docs/mail#postmark-driver" target="_blank">how to configure the Postmark Driver</a> on the Laravel documentation.

`log`
:   Use the mail log channel to send emails.

    Instead of sending your emails, the log mail driver will write all email messages to your log files for inspection. 

    Additional variable is required to be set with this driver:

    - `MAIL_LOG_CHANNEL`
    
    (See accepted values of [LOG_CHANNEL](#log_channel))

`failover`
:   Backup mail delivery configurations that will be used in case your primary delivery driver is down

:::

:::env-var-dl-wrapper

Default value
:   `smtp`

:::

### <span class="expected">MAIL_USERNAME</span>

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Username used to connect to the SMTP server

Default value
:   _none_

:::

### <span class="expected">MAIL_PASSWORD</span>

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Password used to connect to the SMTP server.

    When using `.env` file, if the password contains special characters like `#`, put quotes around it.

Default value
:   _none_

:::

### MAIL_ENCRYPTION

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Encryption protocol used to secure email delivery.

    This var applies only to the `smtp` driver.

Default value
:   `tls`

:::

### <span class="expected">MAIL_HOST</span>

[!badge variant="info" text="string"] [!badge variant="info" text="ip address"] [!badge variant="info" text="domain"]

:::env-var-dl-wrapper

Description
:   Domain of the mail server.

    This var applies only to the `smtp` driver.

Default value
:   `smtp.mailtrap.io`

:::

### <span class="expected">MAIL_PORT</span>

[!badge variant="info" text="number"]

:::env-var-dl-wrapper

Description
:   Port used to communicate with the mail server.

    This var applies only to the `smtp` driver.

Default value
:   `587`

:::

### <span class="expected">MAIL_FROM_NAME</span>

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Name that is used globally for all e-mails that are sent by 2FAuth

Default value
:   `Example`

:::

### <span class="expected">MAIL_FROM_ADDRESS</span>

[!badge variant="info" text="string"] [!badge variant="info" text="email"]

:::env-var-dl-wrapper

Description
:   Address that is used globally for all e-mails that are sent by 2FAuth

Default value
:   `hello@example.com`

:::

### MAIL_VERIFY_SSL_PEER

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v4.2"]

:::env-var-dl-wrapper

Description
:   SSL peer verification.

    !!!warning
    Disabling peer verification may result in a major security flaw. Change it only if you know what you're doing.
    !!!

    This var applies only to the `smtp` driver.

Default value
:   `true`

:::

### MAIL_SENDMAIL_PATH

[!badge variant="info" text="string"] [!badge variant="info" text="path"]

:::env-var-dl-wrapper

Description
:   Path to the sendmail binary

Default value
:   `/usr/sbin/sendmail -bs -i`

:::

## Logs management

The following variables allow you to configure your logging strategy with out-of-the-box options. The <a href="https://laravel.com/docs/10.x/logging" target="_blank">Laravel documentation</a> provides more information on how to configure advanced logging options to meet specific needs.

### LOG_CHANNEL

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   The log channel defines where your log entries go to.

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`daily`
:   Gives you 7 daily rotated log files in `[2FAuth_directory]/storage/logs/`.

    Additional variable may be set to change the number of files:
    
    - [LOG_DAILY_DAYS](#log_daily_days)

`single`
:   Gives you one big fat error log file at in `[2FAuth_directory]/storage/logs/laravel.log`

`errorlog`
:   Writes entries to the error log

`syslog`
:   Writes entries to the system log

    Additional variable may be set to customize the channel:

    - [LOG_SYSLOG_FACILITY](#log_syslog_facility)

`papertrail`
:   Writes entries to a Papertrail instance.

    Additional variables are required to be set:  
    `PAPERTRAIL_URL` URL of the papertrail instance  
    `PAPERTRAIL_PORT` Port used to communicate with the papertrail instance

`slack`
:   Writes entries to a Slack channel.

    Additional variable is required to be set with this channel:

    - [LOG_SLACK_WEBHOOK_URL](#log_slack_webhook_url)

    Additional variables may be set to customize the channel:

    - [LOG_SLACK_USERNAME](#log_slack_username)
    - [LOG_SLACK_EMOJI](#log_slack_emoji)

`stack`
:   A wrapper to facilitate creating "multi-channel" channels.

    Additional variable may be set to define the stack:
    
    - [LOG_STACK](#log_stack)

:::

:::env-var-dl-wrapper

Default value
:   `daily`

:::

### LOG_STACK

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   The stack of log channels used when [LOG_CHANNEL](#log_channel) is set to `stack`.

Accepted values
:   A comma-separated list of valid [LOG_CHANNEL](#log_channel): `daily`, `single`, `errorlog`, `syslog`, `papertrail`, `slack`

    Ex: `slack, papertrail`

Default value
:   `daily`

:::

### LOG_LEVEL

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Determines the minimum "level" a message must be in order to be logged.

    If you set it to `debug` your logs will grow large, and fast. If you set it to `emergency` probably nothing will get logged, ever.

Accepted values
:   From least severe to most severe:

    `debug`, `info`, `notice`, `warning`, `error`, `critical`, `alert`, `emergency`

    See <a href="https://tools.ietf.org/html/rfc5424" target="_blank">RFC 5424</a> for level definitions.

Default value
:   `notice`

:::

### LOG_DAILY_DAYS

[!badge variant="info" text="number"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   Number of log files to generate/rotate when using the `daily` log channel.

Default value
:   `7`

:::

### LOG_SLACK_WEBHOOK_URL

[!badge variant="info" text="string"] [!badge variant="info" text="url"]

:::env-var-dl-wrapper

Description
:   URL of the Slak webhook to use when using the `slack` log channel.

Default value
:   none

:::

### LOG_SLACK_USERNAME

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   The name of the user sending the log messages when using the `slack` log channel.

Default value
:   `Laravel Log`

:::

### LOG_SLACK_EMOJI

[!badge variant="info" text="string"] [!badge variant="info" text="url"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   The Emoji code of the emoji used to illustrate log messages when using the `slack` log channel.

Default value
:   `:boom:`

:::

### LOG_SYSLOG_FACILITY

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   The syslog facility that provides a rough clue of where in a system the message originated.  
    Only applies when log channel is set to `syslog`.

Default value
:   `LOG_USER`

:::

### LOG_DEPRECATIONS_CHANNEL

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   A log channel to use to log the PHP & Laravel deprecation warnings.

    Should match one of the channels described at [LOG_CHANNEL](#log_channel)

Default value
:   `null`

:::

## Proxy setting

### TRUSTED_PROXIES

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   A comma separated IP list of trusted proxies.

    When running 2FAuth behind a proxy that terminates TLS / SSL certificates, you may face some errors. Typically this is because 2FAuth is being forwarded traffic from the proxy on port 80 and does not know it should generate secure links.

    Set to `*` to trust any proxy.

Default value
:   `null`

:::

### PROXY_FOR_OUTGOING_REQUESTS

[!badge variant="info" text="string"] [!badge variant="info" text="url"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   Proxy for outgoing requests, like SSO connection, 2FAuth releases detection or logo fetching.

    You can provide a proxy URL that contains a scheme, username, and password. For example `http://username:password@192.168.16.1:10`.

Default value
:   _blank_

:::

### PROXY_HEADER_FOR_IP

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.2"]

:::env-var-dl-wrapper

Description
:   Name of the HTTP header sent by a reverse proxy to pass the original visitor IP address.

    Check the proxy documentation to find out how this header is named (i.e `HTTP_CF_CONNECTING_IP` for cloudflare).

Default value
:   `null`

:::

## Security setting

### HASH_DRIVER

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   The hash algorithm used to hash user passwords.

    !!!warning
    Changing the hash driver while some users have already registered will break authentication. This var must be set before any user registration.
    !!!

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`bcrypt`, `argon`, `argon2id`

:::

Default value
:   `bcrypt`

:::

### BCRYPT_ROUNDS

[!badge variant="info" text="number"]

:::env-var-dl-wrapper

Description
:   Number of rounds when passwords are hashed using the Bcrypt algorithm.

    Increasing this up to 12 or even 13 will benefit to password security. Be careful, a higher value may significantly affect performance.

Default value
:   `10`

:::

### ARGON_MEMORY

[!badge variant="info" text="number"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   Maximum memory (in kibibytes) that may be used to compute an Argon2 hash.

Default value
:   `65536`

:::

### ARGON_THREADS

[!badge variant="info" text="number"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   Number of threads that Argon2 will use to compute a hash.

Default value
:   `1`

:::

### ARGON_TIME

[!badge variant="info" text="number"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   Maximum amount of time it may take to compute an Argon2 hash.

Default value
:   `4`

:::

## Session setting

### SESSION_DRIVER

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   The default session driver used by 2FAuth to store sessions

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`apc`
:   In-memory key-value store for PHP

`cookie`
:   Sessions are stored in secure, encrypted cookies

`database`
:   Sessions are stored in the database set with [DB_CONNECTION](#db_connection).

    !!!warning
    This driver requires the creation of an additional database table
    !!!

    Additional variables may be set when using this driver:

    - `SESSION_TABLE`

    See <a href="https://laravel.com/docs/session#database" target="_blank">how to configure the Database driver</a> on the Laravel documentation.

`file`
:   Sessions are stored in `[2FAuth_directory]/storage/framework/sessions`

`memcached`
:   Sessions are stored in Memcached.

    !!!warning
    This driver requires the installation of an additional PECL package.
    !!!
    
    See [CACHE_DRIVER](#cache_driver)

`redis`
:   An in-memory data structure store, used as a database, cache, and message broker (<a href="https://redis.io/" target="_blank">redis.io</a>)

    !!!warning
    This driver requires the installation of an additional package.
    !!!
    
    See <a href="https://laravel.com/docs/cache#redis" target="_blank">how to configure the Redis driver</a> on the Laravel documentation.

`dynamodb`
:   Serverless, NoSQL, fully managed database with single-digit millisecond performance at any scale (<a href="https://aws.amazon.com/dynamodb/" target="_blank">dynamodb</a>)

    !!!warning
    This driver requires the creation of an additional database table and the installation of the AWS SDK php package.
    !!!
    
    See <a href="https://laravel.com/docs/cache#dynamodb" target="_blank">how to configure DynamoDB driver</a> on the Laravel documentation.

`array`
:   sessions are stored in a PHP array and will not be persisted

:::

:::env-var-dl-wrapper

Default value
:   `file`

:::

### SESSION_CONNECTION

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   When using the `database` or `redis` [session drivers](#session_driver), you may specify a connection that should be used to manage these sessions.

    This should correspond to a connection in your [database configuration](#db_connection) options.

Default value
:   _none_

:::

### SESSION_STORE

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   While using one of the framework's cache driven session backends you may list a cache store that should be used for these sessions.

    This value must match with one of the 2FAuth's configured cache stores.

Accepted values
:   `apc`, `dynamodb`, `memcached`, `redis`

    (see [CACHE_DRIVER](#cache_driver))

Default value
:   _none_

:::

### SESSION_TABLE

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   Name of the table to be used to store sessions when using the `database` session driver.  
    See [SESSION_DRIVER](#session_driver).

Default value
:   `sessions`

:::

### SESSION_COOKIE

[!badge variant="info" text="string"]

:::env-var-dl-wrapper

Description
:   Name of the cookie used to identify a session instance by ID.

    The name specified here will get used every time a new session cookie is created by 2FAuth for every driver.

Default value
:   `2fauth_session`

:::

### SESSION_DOMAIN

[!badge variant="info" text="string"] [!badge variant="info" text="domain"]

:::env-var-dl-wrapper

Description
:   Domain of the cookie used to identify a session in 2FAuth.

    This will determine which domains the cookie is available to in 2FAuth.

Default value
:   Fallbacks to the 2FAuth instance domain

:::

### SESSION_SECURE_COOKIE

[!badge variant="info" text="boolean"]

:::env-var-dl-wrapper

Description
:   By setting this option to true, session cookies will only be sent back to the server if the browser has a HTTPS connection.

    This will keep the cookie from being sent to you when it can't be done securely.

Default value
:   `false`

:::

### SESSION_ENCRYPT

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   Whether or not session data are encrypted before it is stored.

Default value
:   `false`

:::

## SSO setting

See [Single Sign-On (SSO)](/security/authentication/sso/) to discover how to enable SSO on your instance.

### OPENID_AUTHORIZE_URL

[!badge variant="info" text="string"] [!badge variant="info" text="url"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   URL  used during the OpenID SSO flow to request the user's authentication and consent

Default value
:   _none_

:::

### OPENID_TOKEN_URL

[!badge variant="info" text="string"] [!badge variant="info" text="url"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   URL  used during the OpenID SSO flow to obtain an ID and / or access token

Default value
:   _none_

:::

### OPENID_USERINFO_URL

[!badge variant="info" text="string"] [!badge variant="info" text="url"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   URL used during the OpenID SSO flow to retrieve profile information and other attributes for a logged-in end-user

Default value
:   _none_

:::

### OPENID_CLIENT_ID

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   A unique identifier for the application

Default value
:   _none_

:::

### OPENID_CLIENT_SECRET

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   Secret known only to 2FAuth and the Open ID authorization server

Default value
:   _none_

:::

### OPENID_HTTP_VERIFY_SSL_PEER

[!badge variant="info" text="boolean"] [!badge variant="info" text="string"] [!badge variant="info" text="since v5.6"]

:::env-var-dl-wrapper

Description
:   SSL peer verification during OpenID authentication process

    !!!warning
    Disabling peer verification may result in a major security flaw. Change it only if you know what you're doing.
    !!!

Possible values
:   `true`, `false` or a string representing the path to a custom certificate on disk, like `/path/to/cert.pem`

Default value
:   `true`

:::

### GITHUB_CLIENT_ID

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   A unique identifier for the application

Default value
:   _none_

:::

### GITHUB_CLIENT_SECRET

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   Secret known only to 2FAuth and Github

Default value
:   _none_

:::
