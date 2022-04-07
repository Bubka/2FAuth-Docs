---
order: 100
---

# Authentication

2FAuth has been imagined for personal use only, it is single user designed.  
You have to create a user account to use the app and you cannot create more than one user account. None of the app's features can be used unless you have been authenticated with one of the following methods.

## Login & Password

Authentication in done by submitting your credentials, an email and a password, to the 2FAuth login form. Nothing special here, it is a very common and well-known method.

This is the default authentication method.

## WebAuthn

2FAuth supports the W3C _Web Authentication_ API aka WebAuthn (<a href="https://webauthn.guide/" target="_blank">learn more</a>). This means you can register a security device like a Yubikey, a Titan Security Key or a facial recognition system like Apple FaceId and use it to log into 2FAuth.

This method is considered more secured as it proves you are in fact you because you have to physically own the security device.

!!!
The WebAuthn authentication does not use login & password to sign in but the creation of a user account with an email and a password remains mandatory.
!!!

!!!
WebAuthn is available as an alternative or a replacement to the login/password method. Consider using WebAuthn only to provide the best protection to your 2FAuth instance.
!!!

### Configuration

You can choose

WEBAUTHN_USER_VERIFICATION', 'preferred

### Registering a security device

2FAuth offers to register a WebAuthn device right after submitting the user registration form or through its `Settings > Webauthn` section. Whatever you choose, the registration process will be the same and depends on the hardware you use (desktop, laptop or smartphone) and how your browser implements the WebAuthn flow.

A typical workflow would be:

1. You click the _Register a new device_ button in 2FAuth
2. Your browser prompts you to grant the operation
3. You put your finger on the key's touch button
4. 2FAuth registers the key and offers you to rename it

You can register several security devices, there is no limitation in the number of devices.

### Revoking a security device

You can revoke any registered security device through the `Settings > Webauthn` section.  
Simply click the relevant [!badge variant="dark" text="Revoke"] button.

!!!danger
The revocation of a device is permanent and cannot be undone.
!!!

### I lost my device

Don't worry, there is always a solution, depending on how you have configured the 2FAuth's WebAuthn options.  

+++ WebAuthn is the unique login method

If you have registered another device and still own it just use it to log in. Otherwise you can recover your account by registering a new security device.

Click the __Recover your account__ link of the 2FAuth's login form, this will send a link to your registered email address. Follow this link, you will be able to register a new device and revoke all existing ones.

+++ WebAuthn is NOT the unique login method

Assuming you haven't lost your password too, switch the 2FAuth login form using the link __Sign in using login & password__ and log in using your email address and password, it's that simple!

!!!warning
Don't forget to revoke the lost device in the `Settings > Webauthn` section.
!!!

+++

## Personal Access Token

The only purpose of the Personal Access Tokens (PAT) is to authenticate requests made to the 2FAuth REST API. You can manage your PAT in the `Settings > OAuth` section.

More informations [here](/api/#authentication).

## Authentication proxy

You can configure 2FAuth to let a HTTP proxy handle authentication. In this case 2FAuth will consider you logged in as long as you are authenticated at proxy level. This is particulary usefull if you want to deploy 2FAuth behind a service like <a href="https://sandstorm.io/" target="_blank">Sanstorm</a> or behind an Auth server like <a href="https://www.authelia.com/docs/" target="_blank">Authelia</a>.

2FAuth will check for a HTTP header, named `REMOTE_USER` by default, in every requests from the proxy. (see <a href="https://datatracker.ietf.org/doc/html/rfc3875#section-4.1.10" target="_blank">RFC3875</a>)

### Enable the proxy guard

Set the `AUTHENTICATION_GUARD` environment variable to `reverse-proxy-guard`.  
This can be achieve easily by adding `AUTHENTICATION_GUARD=reverse-proxy-guard` to your `.env` file.

!!!
WebAuthn and Personnal Access Token management is disabled when using the `reverse-proxy-guard`
!!!

!!!warning
Be sure 2FAuth is reachable only through your auth proxy when using the `reverse-proxy-guard` because it will do no further check than the header presence.
!!!

### Customize the header name

You can customize the header name by setting the `AUTH_PROXY_HEADER_FOR_USER` environment variable to match a specific proxy configuration. For example, if the proxy header is `2FAUTH-User`, then set `AUTH_PROXY_HEADER_FOR_USER` to `HTTP_2FAUTH_USER`.

### Additional header

You can configure 2FAuth to check for an additional header that contain the authenticated user email address. This header may or may not exists depending on the auth proxy configuration. The environment variable to set for that is `AUTH_PROXY_HEADER_FOR_EMAIL`.

!!!
For now 2FAuth does not need this information because there is no feature that uses it.
!!!
