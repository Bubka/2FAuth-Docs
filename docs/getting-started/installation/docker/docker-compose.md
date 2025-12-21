# Docker-Compose

## Basic setup

You can run 2FAuth with [Docker-Compose](https://docs.docker.com/compose/) using the following command and this basic `docker-compose.yml` file:

```bash
docker-compose up
```

:::code source="./../../../static/docker-compose.yml" title="`docker-compose.yml`" language="yaml":::

In this example, the `env_file` attribute specifies the `.env` file containing all your environment variables. Please refer to the [Environment variables page](/getting-started/config/env-vars/) to discover all available variables. Be sure to set the mandatory ones marked with a red asterisk.

Another way to set your environment vars is to add them to the `environment` attribute :

```yml:highlight="9-14"
services:
  2fauth:
    image: 2fauth/2fauth:${TAG:-latest}
    container_name: 2fauth
    volumes:
      - ./2fauth:/2fauth
    ports:
      - 8000:8000/tcp
    environment:
      APP_URL: https://2fauth.domain.com
      APP_KEY: "base64:ppkbvJDIlKDv+HgCcgqHtN1gcvbYCDDSU7Y1Y0wBwqg="
      APP_ENV: production
      DB_CONNECTION: sqlite
      DB_DATABASE: "/srv/database/database.sqlite"
```

!!!warning Mandatory env var
The [`APP_KEY`](/getting-started/config/env-vars/#app_key) environment variable must be set with a personal unique value. You can generate a brand new one using [Laravel Encryption Key Generator](https://laravel-encryption-key-generator.vercel.app/).  

If you need to rotate the key, use the [`APP_PREVIOUS_KEYS`](/getting-started/config/env-vars/#app_previous_keys) environment variable to list previous keys and avoid decryption issues or invalid access tokens.
!!!

---

## Using Docker secrets

Since 2FAuth v6.0, sensitive data like passwords may be handled using Docker secrets. The docker image supports the `_FILE` convention for environment variables that receive sensitive data from a bind mount secret.

||| Supported variables
`APP_KEY_FILE`  
`DB_DATABASE_FILE`  
`DB_USERNAME_FILE`  
`DB_PASSWORD_FILE`  
`DB_HOST_FILE`  
`MAIL_USERNAME_FILE`  
`MAIL_PASSWORD_FILE`  
`REDIS_PASSWORD_FILE`
|||

In the following example, `APP_KEY_FILE` is set using the injected `app_key` docker secret that read the content of the `app_key.txt` file. At run time, the `APP_KEY` environment variable will be set automatically using the value of `APP_KEY_FILE`.

```yml:highlight="12,16-17,19-21"
services:
  2fauth:
    env_file: settings.env
    image: 2fauth/2fauth:${TAG:-latest}
    container_name: 2fauth
    volumes:
      - ./2fauth:/2fauth
    ports:
      - 8000:8000/tcp
    environment:
      APP_URL: https://2fauth.domain.com
      APP_KEY_FILE: /run/secrets/app_key
      APP_ENV: production
      DB_CONNECTION: sqlite
      DB_DATABASE: "/srv/database/database.sqlite"
    secrets:
      - app_key

secrets:
  app_key:
    file: /2fauth/app_key.txt
```

!!!warning
You cannot set both the reference variable, for example `APP_KEY`, and its suffixed version, in that case `APP_KEY_FILE`.  
The container will throw an error at run time if both vars are set.
