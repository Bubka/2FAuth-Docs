---
label: 2FA Sharing
---

# 2FA sharing

Starting from v7, 2FAuth allows users to share their 2FA accounts with other users. This lets multiple people access an online service or website with a single user account that's protected by 2FA. It can make life easier in many situations, such as accessing a provider system for a whole team, managing a digital service for a family member who isn't very tech-savvy, or sharing access with a friend.

The 2FA account sharing feature can be globally enabled or disabled by an administrator. See [Administration](/usage/administration/#2fa-sharing).

---

## What is shared

### Reminder on 2FA Secret Confidentiality

TOTP/HOTP 2FA systems rely on a cryptographic secret associated with an account. This secret is used to generate one-time authentication codes that are accepted by the target service during the login process. The security of the entire 2FA mechanism depends directly on the confidentiality of this secret.

For this reason, the 2FA secret must remain strictly confidential at all times:

* It must never be transmitted to unauthorized users.
* It must never be copied outside controlled and secured workflows.
* Access to it must always be restricted and auditable.

### Sharing Without Revealing the Secret

The purpose of the 2FAuth sharing feature is **not** to distribute or expose 2FA secrets between users.

Instead, the application provides a controlled sharing model where authorized users can:

* View the information necessary to identify shared accounts.
* Generate valid 2FA codes for those accounts when required.

At no point does the application disclose the underlying 2FA secret to the users with whom an account is shared.

{.clean .compact}

| Capability                               | User with whom 2FA account is shared | 2FA account owner   |
| ---------------------------------------- | ------- | ------- |
| View account identification information  | Yes     | Yes      |
| Generate valid 2FA codes                 | Yes     | Yes      |
| View or export the underlying 2FA secret | No      | Yes      |
| Retrieve the original seed or QR code    | No      | Yes      |

### Security Model

The feature is designed so that users consume a secure 2FA generation service rather than gaining possession of the secret itself or full control over the 2FA account.

This approach provides several security benefits:

* The confidentiality of the 2FA secret is preserved.
* The risk of uncontrolled duplication is minimized.
* Access can be revoked without rotating the original secret.
* Administrative control and auditing are centralized.
* Shared operational access is possible without weakening the authentication chain.

---

## Sharing Scopes

The application allows 2FA accounts to be shared using one of this sharing scopes:

* `SPECIFIC_USERS`
* `ALL_USERS`

Only one scope can be applied to a shared account at a given time. Multiple accounts can be shared, each with its own scope.

### SPECIFIC_USERS Scope

The `SPECIFIC_USERS` scope allows the owner of a 2FA account to explicitly grant access to one or more selected users to the account. If the owner has several accounts, each of them can be shared individually with different users using this scope.

To simplify recipient discovery, the application provides two search methods:

* **Username search** using partial match capabilities.
* **Email search** using exact match only.

This mode enables fine-grained access control by limiting 2FA code generation capabilities to a defined subset of users.

!!!
2FA accounts can only be shared with users who are already registered on the 2FAuth instance. It is not possible to share an account with an external user.
!!!

### ALL_USERS Scope

The `ALL_USERS` scope grants access to every user of the instance.

This mode is dynamic, meaning that access automatically applies to:

* All current users.
* All future users created on the instance.

No additional configuration is required when new users are added to the platform.

!!!
Because of its broad impact, this sharing scope should be used only when organization-wide access is intentionally required.
!!!

---

## Ownership and Share Management

Only the owner of a 2FA account is authorized to manage its sharing configuration.

The account owner exclusively retains the ability to:

* Create shares.
* Modify sharing scopes.
* Add or remove specific users.
* Revoke existing shares.

Users receiving access to a shared account cannot delegate, modify, or revoke sharing permissions themselves.

---

## Identification of Shared Accounts

Shared accounts are visually identified throughout the application using dedicated badges, allowing you to immediately distinguish ownership and sharing status.

||| Shared By Me
2FA Accounts that you share with others are marked with a sharing badge whose icon reflects the configured sharing scope:

* The [!badge variant="ghost" icon="lucide-user-check"] badge indicates a `SPECIFIC_USERS` share;
* The [!badge variant="ghost" icon="lucide-users"] badge indicates an `ALL_USERS` share.

||| Shared With Me
2FA Accounts that have been shared with you by another account owner are identified using a dedicated badge displaying an [!badge variant="ghost" text="@"] icon.

The badge expands to [!badge variant="ghost" text="@username"] when switching to **Manage** mode.
|||

### Predefined Groups

The application provides two predefined Groups to facilitate navigation and filtering:

* Accounts **shared by me**.
* Accounts **shared with me**.

These groups allow users to quickly access the corresponding subsets of 2FA accounts without visually relying on badges.

!!!tip
When the **Manage** mode is enabled on one of these groups, 2FAuth automatically adapts the available actions according to the permissions associated with each account.

For example:

* Owners can manage or revoke shares on accounts they own.
* Recipient users can only perform actions permitted by their delegated access level.

This behavior ensures that management capabilities always remain consistent with the underlying ownership and sharing model.
!!!

---

## Account Ownership Transfer

The application allows the ownership of a 2FA account to be transferred from one user to another in a controlled and secure manner.

**This only applies when the Sharing feature is enabled.**

### Requirement

Ownership
: Only the current owner of a 2FA account is authorized to initiate and complete an ownership transfer.

  Users benefiting from shared access to an account are never allowed to transfer ownership themselves, regardless of the sharing scope of the account.

Secured transaction
: Because ownership transfer is a sensitive operation, the request must be explicitly validated by the current owner through password confirmation.

### Impact on Existing Shares

When ownership is transferred, the existing sharing configuration remains unchanged.

Any active shares continue to apply under the exact same conditions after the transfer:

* `SPECIFIC_USERS` shares remain assigned to the same users.
    !!!warning
    if the account was shared using the `SPECIFIC_USERS` scope, the previous owner immediately loses access to the account after the transfer is completed.

    The former owner can only regain access if the new owner subsequently creates a new share granting access back to them.
    !!!
* `ALL_USERS` shares continue to apply instance-wide.

### Email Notification

Once the transfer is completed, the new owner automatically receives an email notification informing them that ownership of the 2FA account has been transferred to them.
