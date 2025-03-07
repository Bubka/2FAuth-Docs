---
order: 1000
---

# Administration

## Admin role

The very first account created is automatically set up as an administrator account. Administrators have access to a dedicated area where they can manage global application settings as well as the user base. Click on the _Admin_ link in the 2FAuth footer to access it.

### Granted permissions

Administrators can __consult__, __create__, __promote__, __manage__ or __delete__ any user account.

The account details visible to an administrator include:

- The username
- The email address
- The user ID
- The number of [Personal Access Tokens](/security/authentication/pat/)
- The number of [WebAuthn security devices](/security/authentication/webauthn/)
- The user preferences
- The time the user registered and last visited

When the user registered using SSO:

- The SSO provider
- The user's ID on the provider side

!!!
Although administrators can view information on users, they cannot generate OTPs or even view users's 2FA.
!!!

### Promote to administrator

Any user may be promoted to administrator by another administrator. Edit the user account at _Admin > Users > [User] >_ [!button size="xs" variant="ghost" text="Manage"] and check the [!badge size="l" icon="checkbox" text="Is administrator"] flag. The change is effective immediately, without notification to the promoted user. Demoting is done the same way.

!!!warning
There must always be at least one administrator. The last administrator account cannot demoted.
!!!

An administrator account is identified as such by a banner in the _Settings > Account_ section.

:::mobile-screen
![The administrator banner](/static/admin_account_banner.png)
:::

## Application setup

In addition to environment information, the _Admin > App Setup_ page provides administrators with a number of features for managing the instance.

### Version checking

2FAuth can automatically check if a new version has been released. When enabled, a request will be made to GitHub every week to retrieve the latest version number. You can also run the check manually by clicking the [!button size="xs" corners="pill" text="Check now"] button.

A new available version is reported to the administrators in the 2FAuth footer and the _App Setup_ page.

:::mobile-screen
![The new version indicator in the 2FAuth footer](/static/new_version_available.png)
:::

:::mobile-screen
![The new version alert in the _App Setup_ page](/static/new_version_available_appsetup.png)
:::

!!!
This feature makes outgoing requests that you may want to pass through a proxy.

If so, set the [PROXY_FOR_OUTGOING_REQUESTS](/getting-started/configuration/env-vars/#proxy_for_outgoing_requests) environment variable.
!!!

### Email testing

2FAuth requires a valid email configuration to send emails to users. Features like password reset will not work otherwise.

Click the [!button variant="primary" icon="paper-airplane" iconAlign="left" corners="pill" text="Send" size="xs"] button to send a test email. The email will be sent to your registered email address.

!!!warning
2FAuth does not report whether a test email was successfully sent or not.

Check your email inbox first. If the email is not received, [check your logs](/getting-started/troubleshooting/#check-logs) to get information on the issue.
!!!

### Security

See [Data protection](/security/data-protection/#for-administrators).

## Authentication

### Single Sign-On

See [SSO](/security/authentication/sso).

### Registration control

It is possible to restrict user registration to a limited range of email addresses or to completly disable registrations.

#### Restriction

This is an authorization pattern, only email addresses that meet a condition are allowed to register.

Once the [!badge size="l" icon="checkbox" text="Restrict registration"] setting is enabled in _Admin > App Setup_, there are 2 ways to define the registration policy:

The filtering list
:   Email addresses from this list are allowed to register on 2FAuth.

    Separate the addresses with a `|`. All must be valid email addresses. Ex: `john@example.org|jane@example.net`

    Leave the field blank to disable the filter.

The filtering rule
:   Email addresses that match a regular expression are allowed to register on 2FAuth.

    For example, here is the regex to allow registering using any `@example.org` email address :
    
    `^[A-Za-z0-9._%+-]+@example\.org`

    Leave the field blank to disable the filter.

!!!
Both filtering options can be used simultaneously. The OR operator is applied, this means that the address only has to match one of the conditions to be allowed.
!!!

!!!
The registration policy does not affect SSO.
!!!

#### No registration

Check the [!badge size="l" icon="checkbox" text="Disable registration"] setting to fully disable registration. This affects SSO, so new users won't be able to sign in via SSO.

Check the [!badge size="l" icon="checkbox" text="Keep SSO registration enabled"] setting to override this behavior. New users will be able to sign in for the first time using SSO whereas registration is disabled.

## Users management

### User creation

Administrators can create new user account. Go to _Admin > Users_ and click the [!badge size="l" icon="plus-circle" text="Create a user"] button.

The form provides the exact same fields that a visitor would see in the registration form, with the same validation rules. An additional checkbox is available to directly grant administrator rights to the newly created user: [!badge size="l" icon="checkbox" text="Is administrator"]

!!!warning
When an administrator creates a new user, the password is known.  
It could be considered a bad practice, but this gives some flexibility to the administrator to manage its user base the way he wants.

The administrator can always reset the password of the newly created user. See [Access reset](#access-reset) below.
!!!

### Access reset

While users have the ability to manage their access themselves, administrators can also take action to reset user access at _Admin > Users > [User] >_ [!button size="xs" variant="ghost" text="Manage"].

Possible actions:

[!button size="xs" text="Reset password"]
:   Force resets the current user password with a randomly generated new password then sends a password reset email to the user so they can set their own password.

    Using this, you are guaranteed that the user password has been changed. However, the user is free to set a custom password or not. The token bound to the password reset email received by the user has an expiry time of 60 minutes.

    Any previous request for a password reset, from the user or an administrator, will be revoked.

[!button size="xs" text="Resend email"]
:   Sends a new password reset email to the user without modifying their current password.

    This generates a new reset token with an expiry time of 60 minutes, any previous request will be revoked.

[!button size="xs" text="Revoke"] (<abbr title="Personal Access Token">PAT</abbr>)
:   Revokes all of the user's [Personal Access Tokens](/api/#authentication).

    Once their PATs have been revoked, the user will no longer be able to authenticate to the 2FAuth API.

    !!!warning
    This action is irreversible. Revoked tokens are not searchable, cannot be recovered, and cannot be deleted from the 2FAuth pages.
    !!!
    
    If for some reason you need to purge revoked (or expired) tokens, run the following Artisan commands:

    ```bash !#
    php artisan passport:purge --revoked
    php artisan passport:purge --expired
    ```

[!button size="xs" text="Revoke"] (WebAuthn security devices)
:   Revokes all of the user's [WebAuthn security devices](/security/authentication/webauthn/).

    Once their security devices have been revoked, the user will no longer be able to authenticate using WebAuthn.

    If the user has checked the [!badge size="l" icon="checkbox" text="Use WebAuthn only"] option at _Settings > WebAuthn_, revoking their security devices will reset the option so they can log in with their username and password.

    !!!warning
    This action is irreversible. Revoked devices are not searchable and cannot be recovered from the 2FAuth pages.
    !!!

### User deletion

A user account can be deleted by an administrator, even an account with the Admin role. All data associated with the deleted account will also be deleted, including 2FA records, preferences, access tokens and logs.

Click the [!button size="s" variant="danger" text="Delete this user"] button at _Admin > Users > [User] >_ [!button size="xs" variant="ghost" text="Manage"] to perform the delete.

!!!danger
This is not a soft delete. Deleted account cannot be recovered.
!!!

!!!warning
There must always be at least one administrator. The last administrator account cannot be deleted.
!!!

## Health check

2FAuth provides a special URL to check its health: `/up`

This is a very lightweight resource that responds with a `200` HTTP status code when the application is up and running. It can be used to set up a Docker HEALTHCHECK or a Kubernetes HTTPS liveness probe.
