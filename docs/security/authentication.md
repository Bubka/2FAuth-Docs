---
order: 100
---

# Authentication

You have to create a user account to use the app, none of the app's features can be used unless you have been authenticated with one of the methods described below.

## Built-in

Authentication is done by submitting your credentials, an email and a password, to the 2FAuth login form. Nothing special here, it is a very common and well-known method.

This is the default authentication method.

## WebAuthn

2FAuth supports the W3C _Web Authentication_ API aka WebAuthn (<a href="https://webauthn.guide/" target="_blank">learn more</a>). This means you can register a security device like a Yubikey, a Titan Security Key, or a facial recognition system like Apple FaceId and use it to log into 2FAuth.

![image by Arun (dribbble.com/nullpointone)](/static/webauthn_login.gif)

This method is considered more secure, as it proves you are in fact you because you have to physically own the security device.

WebAuthn is available in 2FAuth as a complement or a replacement to the built-in login/password method. Consider using only WebAuthn to provide the highest protection to your 2FAuth instance.

!!!
The WebAuthn flow does not use login & password, but the creation of a user account with an email and a password remains mandatory.
!!!

### Registering a security device

2FAuth offers to register a WebAuthn device right after submitting the user registration form or through its _Settings > Webauthn_ section. Whatever you choose, the registration process will be the same and depends on the hardware you use (desktop, laptop or smartphone) and how your browser implements the WebAuthn flow.

A typical workflow would be:

1. You click the [!badge size="l" icon="plus-circle" text="Register a new device"] link in 2FAuth
2. Your browser prompts you to grant the operation
3. You put your finger on the key's touch button
4. 2FAuth registers the key and offers you to rename it

You can register several security devices, there is no limitation in the number of devices.

### Revoking a security device

You can revoke any registered security device through the _Settings > Webauthn_ section.  
Simply click the relevant [!badge variant="dark" text="Revoke"] button.

!!!danger
The revocation of a device is permanent and cannot be undone.
!!!

### I lost my device

Don't worry, there is always a solution, depending on how you have configured 2FAuth's WebAuthn options.  

+++ WebAuthn is the unique login method

If you have registered another device and still own it, just use this device to log in. Otherwise, you can recover your account by registering a new security device.

:icon-arrow-right: Click the __Recover your account__ link of the 2FAuth's login form, this will send a link to your registered email address. Follow this link, you will be able to register a new device and revoke all existing ones.

+++ WebAuthn is NOT the unique login method

Assuming you haven't lost your password too, switch the 2FAuth login form using the link __Sign in using login & password__ and log in using your email address and password. It's that simple!

!!!warning
Don't forget to revoke the lost device in the _Settings > Webauthn_ section.
!!!

+++

### User verification

Most authenticators and smartphones will ask the user to actively verify themselves to log in. For example, through a touch plus pin code, password entry, or biometric recognition (e.g., presenting a fingerprint). The intent is to distinguish one user from any other.

You can configure how the user verification step behave during the WebAuthn login flow with the `WEBAUTHN_USER_VERIFICATION` env var:

```env In your .env file:
WEBAUTHN_USER_VERIFICATION=preferred
```

Supported value | Behavior { class="compact" }
--- | ---
_required_ | Will ALWAYS ask for user verification
_preferred_ (default) | Will ask for user verification IF POSSIBLE
_discouraged_ | Will NOT ask for user verification (for example, to minimize disruption to the user interaction flow)

## Personal Access Token

Use Personal Access Tokens (PAT) to authenticate requests sent to the 2FAuth REST API.

[!ref How to authenticate API requests](/api/#authentication)

## Authentication proxy

You can configure 2FAuth to let an HTTP proxy handle authentication. In this case, 2FAuth will consider you logged in as long as you are authenticated at proxy level. This is particularly useful if you want to deploy 2FAuth behind a service like <a href="https://sandstorm.io/" target="_blank">Sandstorm</a> or behind an Auth server like <a href="https://www.authelia.com/docs/" target="_blank">Authelia</a>.

2FAuth will check for an HTTP header, named `REMOTE_USER` by default, in every request from the proxy. (see <a href="https://datatracker.ietf.org/doc/html/rfc3875#section-4.1.10" target="_blank">RFC3875</a>)

!!!warning
2FAuth only check for the header presence, nor its validity nor its content, so be sure your instance cannot be reached otherwise than through your auth proxy.
!!!

### Enable the proxy guard

Set the `AUTHENTICATION_GUARD` environment variable to `reverse-proxy-guard` to enable the auth proxy authentication.  

```env In your .env file:
AUTHENTICATION_GUARD=reverse-proxy-guard
```

!!!
WebAuthn and Personal Access Token are not supported when using the `reverse-proxy-guard`
!!!

### Define the header value

The `REMOTE_USER` header can take any value. For 2FAuth, its value is the username of the user account to consider authenticated.

If you already have a user account in 2FAuth, set the `REMOTE_USER` header value (at proxy level) like the _name_ field of your account.

If you do not have a user account yet, or if you want to be authenticated as a brand new user, set the header to a fresh value, 2FAuth will take care of creating the account for you.

### Customize the header name

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

### Additional header

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
