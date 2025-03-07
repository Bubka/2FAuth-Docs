---
order: 100
---
# Single Sign-On (SSO)

> _Single sign-on is an authentication scheme that allows a user to log in with a single ID to any of several related, yet independent, software systems._ ([_Wikipedia, CC Attribution-ShareAlike_](https://en.wikipedia.org/wiki/Single_sign-on))

In other words, you can use an existing account, say your github account, to authenticate to 2FAuth.

SSO is probably overkill for a single user usage but becomes relevant in a multi-user context, especially if your organization already uses OAuth.

!!!info
For now 2FAuth only supports 2 SSO providers: __OpenID__ and __Github__
!!!

## Enabling SSO

SSO is enabled by default. You can check it or change it at _Admin > Auth_.

!!!
SSO makes outgoing requests that you may want to pass through a proxy.

If so, set the [PROXY_FOR_OUTGOING_REQUESTS](/getting-started/configuration/env-vars/#proxy_for_outgoing_requests) environment variable.
!!!

## Enable a provider

### Create the client

You have to create a client ID on the provider side first. This is required so that your 2FAuth instance is considered legit when requesting an access token to the provider.

Please refer to the vendor's documentation for instructions on how to do this. During the process, when/if asked:

- Choose the __Authorization code grant flow__
- Choose the __Web application flow__
- The Authorization callback URL is to build as:  
  `[your_2FAuth_url]/socialite/callback/[the_provider_name]`

> __Example__:  
>_Your 2FAuth instance url is `https://2fauth.mydomain.com`  
>_Your provider is __Github__  
>
> Then your callback URL is:  
> `https://2fauth.mydomain.com/socialite/callback/github`

At the end of the process, you should be provided with a Client ID & Secret. Copy them as they are needed to set up the provider on the 2FAuth side.

#### Usefull resources (for Github)

[!ref icon="mark-github" target="_blank" text="Manage your OAuth apps"](https://github.com/settings/developers)

[!ref icon="book" target="_blank" text="creating-an-oauth-app"](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/creating-an-oauth-app)

### Set up the provider

Setting up a provider is done by defining its dedicated environment variables on your 2FAuth instance. You can find these vars in the `.env` file of 2FAuth. See also the [SSO setting](/getting-started/configuration/env-vars/#sso-setting) section.

```env
# OpenID (enabled)
OPENID_AUTHORIZE_URL=https://samples.auth0.com/authorize
OPENID_TOKEN_URL=https://samples.auth0.com/oauth/token
OPENID_USERINFO_URL=
OPENID_CLIENT_ID=hfQ3kkxs3C45mCsc0fI8lvthuQGr7bqc
OPENID_CLIENT_SECRET=1QzLnSc1Rc3MCPm0QV2FLNaKk-Jk0bUjaCageDvxIkd2-Mp50ipiP-MCPm0QV-Da

# Following provider is disabled
# GITHUB_CLIENT_ID=xhfmb8tp7yo6vivbo1ba
# GITHUB_CLIENT_SECRET=r6t44wdh4jlm5mwso10erbjaux1b6cn1zspb3gk5
```

Uncomment the lines for the providers you want to enable and assign the values with the information you obtained previously during the client creation.

!!!warning
Uncommented providers but with empty `CLIENT_ID` or `CLIENT_SECRET` won't be available.
!!!

## Sign with a provider

Once a provider is enabled, a button to _Continue with_ this provider is available on the 2FAuth's Login page.

:::mobile-screen
![_Continue With_ buttons for SSO](/static/auth_sso_login.png)
:::

Clicking a button will take you through the following steps:

1. You will be redirected to the provider site (where you may need to authenticate)
2. You will be prompted to grant permissions to 2FAuth to access your account
3. Regardless of your choice, you will be redirected back to 2FAuth
4. If you have granted access, you will be authenticated. If not, you will be back to the login form

!!!warning
You cannot sign in via SSO with a provider account that uses an email already registered on 2FAuth. Accounts cannot be merged.
!!!

### Registered without registering

When you sign in via SSO for the first time, you are registered to 2FAuth transparently. This means you own a 2FAuth user account on the instance but this account is bound to the provider account with the following restrictions:

- You won't be provided with a password, but the reset password feature will apply if you need one (e.g. to delete your account)
- The 2FAuth account cannot be unbound from the provider account.
- You cannot change your information from 2FAuth. But changes made on the provider side are reflected on 2FAuth each time you sign in via SSO.

## Use SSO only

SSO can be set as the only authentication method available on your 2FAuth instance. Go to _Admin > Auth_, scroll down to the _Single Sign-On_ section and check [!badge size="l" icon="checkbox" text="Use SSO only"].

Enabling this setting has the following effects:

- Most authentication features are disabled for standard users: Password & WebAuthn Login, Password Reset, Registration, [OAuth PAT](/security/authentication/pat/) and [WebAuthn](/security/authentication/webauthn/) devices management.
- Administrators can still log in with their password or WebAuthn. This is a security feature to prevent lockout if no SSO provider is available.

## Disabling SSO

As an administrator, you can fully disable Single Sign-On from the 2FAuth UI.

Go to _Admin > Auth_, scroll down to the _Single Sign-On_ section and uncheck [!badge size="l" icon="checkbox" text="Enable Single Sign-On"].

Note that:

- Existing "SSO users" won't be able to sign in via SSO anymore, but their accounts remain. Still, the password reset feature can be used so they can get a password and sign in again.
- There is no need to unset the providers env vars.

Enabling back SSO restores the providers and the ability for SSO users to sign in again.
