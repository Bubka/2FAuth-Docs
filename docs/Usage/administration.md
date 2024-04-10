---
order: 1000
---

# Administration

## Admin role

The very first account created is automatically set up as an administrator account. Administrators have access to a dedicated area where they can manage global application settings as well as the user base. Click on the _Admin_ link in the 2FAuth footer to access it.

### Granted permissions

Administrators can __consult__, __create__, __promote__, __manage__ or __delete__ any user account.

User/account data visible to administrators are:

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

Any user may be promote to administrator by another administrator. Edit the user account at _Admin > Users > [User] >_ [!button size="xs" variant="ghost" text="Manage"] and check the [!badge size="l" icon="checkbox" text="Is administrator"] flag. The change is effective immediately, without notification to the promoted user. Demoting is done the same way.

!!!warning
There must always be at least one administrator. The last administrator account cannot be deleted or demoted.
!!!

An administrator account is identified as such by a banner in the _Settings > Account_ section.

:::mobile-screen
![The administrator banner](/static/admin_account_banner.png)
:::

## Application setup

In addition to environment information, the _Admin > App Setup_ page provides administrators with a number of features for controlling the instance.

### Version checking

2FAuth can automatically check if a new version has been released. When enabled, a request will be made to GiHub every week to retrieve the latest version number. You can also run the check manually by clicking the [!button size="xs" corners="pill" text="Check now"] button.

A new available version is reported to the administrators in the 2FAuth footer and the _App Setup_ page.

:::mobile-screen
![The new version indicator in the 2FAuth footer](/static/new_version_available.png)
:::

:::mobile-screen
![The new version alert in the _App Setup_ page](/static/new_version_available_appsetup.png)
:::

!!!
This feature makes outgoing requests that you may want to pass through a proxy.

If so, set the [PROXY_FOR_OUTGOING_REQUESTS](/getting-started/configuration/#proxy_for_outgoing_requests) environment variable.
!!!

### Email configuration testing

2FAuth requires a valid email configuration to send emails to users. Features like password reset will not work otherwise.

Click the [!button variant="primary" icon="paper-airplane" iconAlign="left" corners="pill" text="Send" size="xs"] button to send a test email. The email will be sent to your registered email address.

!!!warning
2FAuth does not report whether a test email was successfully sent or not.

Check your email inbox first. If the email is not received, [check your logs](/getting-started/troubleshooting/#check-logs) to get information on the issue.
!!!

## Users management

You can create as many accounts as you like, the only restriction is that each user must have a unique __name__ and __email address__.

### User creation

Administrators can create new user account. Go to _Admin > Users_ and click the [!badge size="l" icon="plus-circle" text="Create a user"] button.

The form provides the exact same fields that a visitor would see in the registration form, with the same validation rules. An additional checkbox is available to directly grant administrator rights to the newly created user: [!badge size="l" icon="checkbox" text="Is administrator"]

!!!warning
When an administrator creates a new user, he is the one who set the user password so the password is not a secret to him.

It could be considered a bad practice, but this gives some flexibility to the administrator to manage its user base the way he wants.

The administrator can always reset the password of the newly created user. Go to  _Admin > Users > [User] >_ [!button size="xs" variant="ghost" text="Manage"] and click the [!button size="xs" text="Reset password"] button
!!!

### Disable user registration

Starting from v4.2.0, you can disable user registration. Go to _Admin > App setup_ and check the [!badge size="l" icon="checkbox" text="Disable registration"] setting.