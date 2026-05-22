# Auth proxy

You can configure 2FAuth to let an HTTP proxy handle authentication. In this case, 2FAuth will consider you logged in as long as you are authenticated at proxy level. This is particularly useful if you want to deploy 2FAuth behind a service like <a href="https://sandstorm.io/" target="_blank">Sandstorm</a> or behind an Auth server like <a href="https://www.authelia.com/docs/" target="_blank">Authelia</a>.

2FAuth will check for an HTTP header, named `REMOTE_USER` by default, in every request from the proxy. (see <a href="https://datatracker.ietf.org/doc/html/rfc3875#section-4.1.10" target="_blank">RFC3875</a>)

!!!warning
2FAuth only check for the header presence, nor its validity nor its content, so be sure your instance cannot be reached otherwise than through your auth proxy.
!!!

---

## Enable the proxy guard

Set the `AUTHENTICATION_GUARD` environment variable to `reverse-proxy-guard` to enable the auth proxy authentication.  

```env In your .env file:
AUTHENTICATION_GUARD=reverse-proxy-guard
```

!!!
WebAuthn and Personal Access Token are not supported when using the `reverse-proxy-guard`
!!!

---

## Define the header value

The `REMOTE_USER` header can take any value. For 2FAuth, its value is the username of the user account to consider authenticated.

If you already have a user account in 2FAuth, set the `REMOTE_USER` header value (at proxy level) like the _name_ field of your account.

If you do not have a user account yet, or if you want to be authenticated as a brand new user, set the header to a fresh value, 2FAuth will take care of creating the account for you.

---

## Customize the header name

You can customize the header name by setting the `AUTH_PROXY_HEADER_FOR_USER` environment variable to match a specific proxy configuration. For example, if the proxy header is `2FAUTH-User`, then set `AUTH_PROXY_HEADER_FOR_USER` as such:

```env In your .env file:
# if the proxy header is '2FAUTH-User'
AUTH_PROXY_HEADER_FOR_USER=2FAUTH-User
```

Some proxies may add a prefix to headers, like `HTTP_`. You have to add it to your headers name as well.

```env In your .env file:
# if the proxy prefix is 'HTTP_'
AUTH_PROXY_HEADER_FOR_USER=HTTP_2FAUTH-User
```

---

## Additional header

You can configure 2FAuth to check for an additional header that contain the authenticated user email address. This header may or may not exist depending on the auth proxy configuration. Its name should be declare using the environment variable `AUTH_PROXY_HEADER_FOR_EMAIL`.

```env In your .env file:
# if the proxy pushes a header named REMOTE_USER_EMAIL
AUTH_PROXY_HEADER_FOR_EMAIL=REMOTE_USER_EMAIL
```

As long as the header is sent by the proxy, its value will be used by 2FAuth as the user email address.

!!!warning
The email passed through the header must be a valid and unused email. If a 2FAuth user already uses this email, 2FAuth will ignore it.
!!!

If the header is no longer sent (or is ignored), the user's email will be fallbacked to a fake `@remote` email adress by 2FAuth.
