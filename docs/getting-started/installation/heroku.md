# Heroku

You can deploy 2FAuth to Heroku by clicking the below button:

[--![](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/Bubka/2FAuth)

<div style="clear: both;"></div>

## Restrictions

Heroku use an <a href="https://devcenter.heroku.com/articles/dynos#ephemeral-filesystem" target="_blank">ephemeral filesystem</a> which means 2FAuth cannot use this file system to store and retrieve the 2FA icons across multiple sessions. For now the only workaround is to disable icon display in 2FAuth:

:icon-arrow-right: Uncheck the [!badge size="l" icon="tasklist" text="Show icons"] option in the 2FAuth's _Settings > Options_ section

## Configuration

Although the Heroku button tends to ease the installation process you have to configure some environment variables when creating the apps at Heroku.

### Security keys

2FAuth needs an RSA key pair for Laravel Passport to generate security tokens. Normally these keys are generated automatically and stored in a 2FAuth subfolder but the Heroku ephemeral filesystem breaks this behaviour. You have to generate the keys on your own and then set the corresponding env vars.

#### Generate RSA keys

Run the following command to generate a private `private.pem` RSA key:

```sh
openssl genrsa 
```

Then derivates a public key `public.pem` from the newly created `private.pem` key:

```sh
openssl rsa -in private.pem -pubout -out public.pem
```

!!!
Despite this is not a good security practice, you can also use an online generator like <https://cryptotools.net/rsagen>
!!!

#### Set Heroku env vars

:icon-arrow-right: Set the `PASSPORT_PRIVATE_KEY` environment var with the content of the `private.pem` file.  
:icon-arrow-right: Set the `PASSPORT_PUBLIC_KEY` environment var with the content of the `public.pem` file.

They should look like this:

+++ Public key

```text
-----BEGIN PUBLIC KEY-----
MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEA4gvRXnfAg7Tve9tZ0NVw
gUXvjevT6bz/n1ujVkWApxPBJNXKBp7MCNkNSJ+zuraqxiQo+zAMPF9DiLqT3MjS
urhmKJdQxAbC9xn/JLiSc5oFojR1ARodReo0f0TRl91pmuPxIEbWurWdmjj3/PpA
DthJ+cSTVmkpoienFkkVOooVdcL86ryA4D9vHZpX/peLpfpwhuuVP//6Egt59a4s
cyOJFK1773COcYT+ssKawollV0p/UgW19jnzHtPlRS77Y8TA7sj6hOO5Qgqv+c0e
xMxkIGCRBT50CHUnoInsEE+Uofzaihe40FF2KDpCAp+AQeL+/W+CHd3AEZsxPlqy
qbEi5onG+9CUWBJzqYRaec6WtxFEICYC1wJ4nGNZO9CaZ9aA5Y8PZMsON32weI2q
Z2hK2v48ucPYRV3dO/H8+xokX73U4dBJT+GU/e8rctWeewnqi9tPc2S2oQs7v3PS
LlGtChc5PFetT+doUE8m/JVvkyT3JzYkOM7FoLnk8sGDpAsNOiZKlIXk30J8crx1
bzioWbthShwQWqBphYwRXvODcr0c8skLa5hna3EoMZBphCAjL413LKartHkZPTG+
5pOQKDH3lv4lcSj0i0s596as8GlV75Jz1fuCdyDCOZxzA6sBlPikgGUrmLzSGM5B
XIsHWhOzzxDiWHihtymsLlUCAwEAAQ==
-----END PUBLIC KEY-----
```

+++ Private key

```text
-----BEGIN RSA PRIVATE KEY-----
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
-----END RSA PRIVATE KEY-----
```

+++

!!!warning
Do NOT use those keys, they are provided as an example
!!!

### Email

When using the [!badge Deploy to Heroku] button, you will be prompted to set email vars for a basic SMTP configuration.  
It will be appropriate most of the time but this is not the only possible configuration supported by Laravel.

!!!
Email setup is only needed for the 2FAuth 'Recover your password' feature. You can skip it if you just want to try 2FAuth.
!!!

The Email configuration depends on your email provider. You should refer to its documentation to find the relevant values.  
As an example here is the configuration for an OVH hosting using SSL:

```env
MAIL_HOST=SSL0.OVH.NET
MAIL_PORT=465
MAIL_ENCRYPTION=ssl
```

Read more about email configuration at <https://laravel.com/docs/mail>.

## Free resources

2FAuth offers a basic Heroku setup which only use free resources: A single add-ons, `Heroku Postgres` with Hobby plan, and a single `Web` Dyno with `NGINX`. This way any Heroku user can deploy the app, even if the user account is not verified.

Be aware that the Postgres has some limitations, but that should not be a problem as 2FAuth has very few database needs.  
Read more about the Heroku Postgres plans at <a href="https://devcenter.heroku.com/articles/heroku-postgres-plans#hobby-tier" target="_blank">devcenter.heroku.com</a>
