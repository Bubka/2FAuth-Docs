---
order: 90
---
# Multi-user

Since v4.0, 2FAuth is a multi-user app. You can share your instance and let your family members, collaborators or friends create their own account.

## Administrator

The very first account created is automatically sets as an administrator account. For now this role can only manage few settings that apply to every users. These settings can be found at the bottom of the _Settings > Options_ section.

It is not possible for the administrator to manage other user accounts or their 2FA data.

!!!warning
The __Delete account__ feature in _Settings > Account_ cannot apply to the Administrator account.
!!!

The administrator account is identified as such by a banner in the _Settings > Account_ section.

:::mobile-screen
![The administrator banner](/static/admin_account_banner.png)
:::

## Users

You can create as many accounts as you want, the only limitation is that every user must register with a unique __Name__ and __Email address__.

A user has access to their 2FAs only, there is no way to access another account's 2FAs or to share 2FAs between user, even for an administrator.
