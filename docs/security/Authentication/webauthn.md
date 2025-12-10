---
order: 90
---
# WebAuthn (aka Passkey)

2FAuth supports the W3C _Web Authentication_ API aka WebAuthn (<a href="https://webauthn.guide/" target="_blank">learn more</a>). This means you can register a security device like a Yubikey, a Titan Security Key, or a facial recognition system like Apple FaceId and use it to log into 2FAuth.

![image by Arun (dribbble.com/nullpointone)](/static/webauthn_login.gif)

This method is considered more secure, as it proves you are in fact you because you have to physically own the security device.

WebAuthn is available in 2FAuth as a complement or a replacement to the built-in login/password method. Consider using only WebAuthn to provide the highest protection to your 2FAuth instance.

!!!warning
The WebAuthn flow does not use login & password, but the creation of a user account with an email and a password remains mandatory.
!!!

---

## Registering a security device

2FAuth offers to register a WebAuthn device right after submitting the user registration form or through its _Settings > Webauthn_ section. Whatever you choose, the registration process will be the same and depends on the hardware you use (desktop, laptop or smartphone) and how your browser implements the WebAuthn flow.

A typical workflow would be:

1. You click the [!badge size="l" icon="plus-circle" text="Register a new device"] link in 2FAuth
2. Your browser prompts you to grant the operation
3. You put your finger on the key's touch button
4. 2FAuth registers the key and offers you to rename it

You can register several security devices, there is no limitation in the number of devices.

---

## Revoking a security device

You can revoke any registered security device through the _Settings > Webauthn_ section.  
Simply click the relevant [!badge variant="dark" text="Revoke"] button.

!!!danger
The revocation of a device is permanent and cannot be undone.
!!!

---

## I lost my device

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

---

## User verification

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
