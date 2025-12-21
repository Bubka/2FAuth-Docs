# Docker-Compose

You can run 2FAuth with [Docker-Compose](https://docs.docker.com/compose/) using the following command and `docker-compose.yml` file:

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

