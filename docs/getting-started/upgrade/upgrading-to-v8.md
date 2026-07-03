---
title: Upgrading to 2FAuth v8
label: Upgrading to v8
---

## Hight Impact Changes

### OAuth Key files permissions

The component responsible of the authentication process (Passport) now validates the permissions of key files on supported operating systems. The Docker image as well as the [installation wizard](/getting-started/installation/self-hosted-server/#2fauth-set-up-wizard) of 2FAuth v8 already apply the necessary changes automatically but you may face an error page, though.

If you upgrade manually, run the following command at the end of your upgrade process:

```bash
php artisan 2fauth:fix-passport-key-permissions
```

#### Possible Docker issue

When using Docker, you may encounter the following error: `chmod(): Operation not permitted`. This is probably because the ownership of the key files is not correct.

Ensure the `/storage/oauth-private.key` and `/storage/oauth-public.key` files stored in the directory of your bound volume are owned by user:group `1000:1000`.

You may also change the file permissions manually:

- `oauth-private.key` permissions must be set to `0660`
- `oauth-public.key` permissions must be set to `0600`

### Authentication proxy must be trusted

If you are using an authentication proxy in front of 2FAuth (meaning you have `AUTHENTICATION_GUARD=reverse-proxy-guard` in your environment variables), you **must** now declared this proxy as trusted.

Set (or complete) the [`TRUSTED_PROXIES`](/getting-started/config/env-vars/#trusted_proxies) environment variable with the address of your authentication proxy.

---

## Medium Impact Changes

### Personal Access Tokens invalidation

The way authentication tokens are generated has changed due to an update to one of the underlying components, so all your personal access tokens created using 2FAuth v7 or an earlier version will become invalid. They will no longer be visible in *Settings > OAuth > Personal Access Token*.

You will have to [create new tokens](/security/authentication/pat/#creating-an-access-token).

---

## Low Impact Changes

### New environment variables

The new `PHP_MEMORY_LIMIT_TEMP_OVERRIDE` environment variable lets you control the maximum amount of memory PHP can consume temporarily during QR Code detection operations.

See [`PHP_MEMORY_LIMIT_TEMP_OVERRIDE`](/getting-started/config/env-vars/#php_memory_limit_temp_override).
