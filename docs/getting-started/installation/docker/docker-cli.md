---
order: 90
---
# Docker CLI

An official Docker image is available to run 2FAuth in a single Docker container.
These are the __Docker CLI__ Setup instructions, a <a href="https://github.com/Bubka/2FAuth/blob/master/docker/docker-compose.yml" target="_blank">docker-compose.yml</a> file is also available.

[--![](https://dockeri.co/image/2fauth/2fauth)](https://hub.docker.com/r/2fauth/2fauth)

<div style="clear: both;"></div>

## Features

- [![](https://img.shields.io/docker/image-size/2fauth/2fauth/latest)](https://hub.docker.com/r/2fauth/2fauth/tags)
- Compatible with: [!badge amd64], [!badge 386], [!badge arm64], [!badge arm/v6] and [!badge arm/v7]
- Stores data in a Sqlite database file
- Runs without root as user with id `1000` and group id `1000`

## Tags

Several Docker tags are available to let you choose which version you want to run:

| Tags | Description | { class="compact" }
| --- | --- | --- |
| `latest` | The current state of the master branch.<br />Considered stable. May include some fixes/changes not yet officially released. |
| Release tags: `3.0.2` `3.1.0` ... `x.y.z` | The version at a corresponding [GitHub release](https://github.com/Bubka/2FAuth/releases).<br />Considered stable, and frozen. |
| `dev` | The current state of the dev branch<br />May be unstable or even broken.

Simply append the tag name to the docker image name in your command or script, separated by a colon, to specify which tag to use. e.g `2fauth/2fauth:3.0.2`, or `2fauth/2fauth:dev`.

If no tag is specified, Docker will default to `latest`.

## Docker CLI Setup

We assume your current directory is `/yourpath`.

1. Create a directory on your host

    ```sh
    mkdir 2fauth
    ```

    !!!warning If your host is not Windows
    Since the container runs without root as user `1000:1000`, you need to fix the ownership and permissions of that directory:

    ```sh
    chown 1000:1000 2fauth
    chmod 700 2fauth
    ```

    If you feel like using another ID, you can [build the image with build arguments](#build-the-image-with-build-arguments).
    !!!

1. Run the container interactively

    ```sh
    docker run -it --rm -p 8000:8000/tcp \
    -v /yourpath/2fauth:/2fauth 2fauth/2fauth
    -e AUTHENTICATION_GUARD=web-guard
    ```

1. Access it in your browser

    [!ref target="blank" text="localhost:8000"](http://localhost:8000)

You can stop it with `CTRL+C`.

- You can also run it in the background by replacing `-it --rm` with `-d`.
- You can set available environment variables (see the <a href="https://github.com/Bubka/2FAuth/blob/master/.env.example" target="_blank">.env.example</a>) with `-e`, for example `-e APP_NAME=2FAuth`.

### Use an existing SQLite file

If you already have an SQLite file, move it to `/yourpath/2fauth/database.sqlite` on your host before starting the container. Don't forget to fix its ownership and permissions if you run on *nix:

```sh
chown 1000:1000 /yourpath/2fauth/database.sqlite
chmod 700 /yourpath/2fauth/database.sqlite
```

The container will automatically pick it up.

## Build the image

You can build the image from the `master` branch with `docker` and `git` using:

```sh
docker build -t 2fauth/2fauth https://github.com/Bubka/2FAuth.git
```

### Build the image for a specific release

You can build a [specific release](https://github.com/Bubka/2FAuth/releases) by appending the release tag with `#<release-tag>` to the command. For example:

```sh
docker build -t 2fauth/2fauth https://github.com/Bubka/2FAuth.git#v2.1.0
```

### Build the image for a specific commit

You can build a specific commit (see [master's commits](https://github.com/Bubka/2FAuth/commits/master)) by appending the commit hash with `#<commit-hash>` to the command. For example:

```sh
docker build -t 2fauth/2fauth https://github.com/Bubka/2FAuth.git#fba9e29bd4e3bb697296bb0bde60ae869537528b
```

### Build the image with build arguments

Use the following build arguments to customize the image with `--build-arg key=value`:

| Build argument | Default | Description | { class="compact" }
| --- | --- | --- |
| `UID` | 1000 | The UID of the user to run the container as |
| `GID` | 1000 | The GID of the user to run the container as |
| `DEBIAN_VERSION` | `buster-slim` | The Debian version to use |
| `PHP_VERSION` | `7.3-buster` | The PHP version to use to get composer dependencies |
| `COMPOSER_VERSION` | `2.1` | The version of composer to use |
| `SUPERVISORD_VERSION` | `v0.7.3` | The version of supervisord to use |
| `VERSION` | `unknown` | The version of the image |
| `CREATED` | `an unknown date` | The date of the image build time |
| `COMMIT` | `unknown` | The commit hash of the Git commit used |

#### Mail settings

| Build argument | Default | Description |  { class="compact" }
| --- | --- | --- |
| `MAIL_HOST` | `smtp.mailtrap.io` | The SMTP hostname |
| `MAIL_PORT` | 2525 | The corresponding SMTP port |
| `MAIL_FROM` | `changeme@example.com` | The sender address |
| `MAIL_USERNAME` | null | The SMTP username |
| `MAIL_PASSWORD` | null | The SMTP password |

Example:

```sh
...
-e MAIL_HOST=smtp.example.com
-e MAIL_PORT=587
-e MAIL_FROM=2fauth@example.com
-e MAIL_USERNAME=2fauth@example.com
-e MAIL_PASSWORD=password1234
```

## Implementation details

- The final Docker image is based on [!badge alpine:3.14](https://hub.docker.com/_/alpine) with minimal packages installed
- The container runs [`supervisord`](https://github.com/ochinchina/supervisord) to handle both an Nginx server and a PHP-FPM server together
- The `/srv` directory holds the repository data and PHP code.
- The `/2fauth` directory is targeted for the container end users.
- By default, the container logs the Nginx logs and the PHP-FPM logs. The application logs (if any) can be found in `/2fauth/storage/logs`.
