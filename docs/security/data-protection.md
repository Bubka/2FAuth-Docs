---
order: 90
---
# Data protection

2FAuth provides several security mechanisms to protect your sensitive 2FA data, at administrator or user level.

## For the administrator

### DB encryption

Sensitive data stored in the database (2FA secret & otpauth URI) can be encrypted to protect them against a database compromise.

:icon-arrow-right: Check the [!badge size="l" icon="checkbox" text="Protect sensible data"] option in the 2FAuth's _Settings > Options_ section to enable encryption.

!!!
Encryption applies to all users data
!!!

!!!warning Warning
It is strongly recommended to backup the `APP_KEY` value defined in your .env file (or the whole file) when encryption is enabled.

__There is no way to generate One-Time Password if you lose this key.__  
__There is no workaround in case of key loss.__
!!!

## For users

### Auto lock

2FAuth can automatically log you out to keep your data always protected. The goal is to avoid a long life session that someone could reuse, for example from a public computer you forgot to clean or from your own stolen smartphone.

Supported trigger | Behavior { class="compact" }
--- | ---
_On security code copy_ | You will be logged out immediately after you click/tap on a One-Time Password to copy it
_a time lapse_ | You will be logged out after a certain amount of time
_Never_ | Disable the Auto lock

:icon-arrow-right: Use the [!badge size="l" icon="single-select" text="Auto lock"] combobox in the 2FAuth's _Settings > Options_ section to select a trigger or to disable the feature.

### Sensitive data hiding

You can configure 2FAuth to display obfuscated One-Time Password rather than human readable password.

Without obfuscation | With obfuscation
--- | ---
377 609 | ●●● ●●●

This protects against attacks like a _shoulder-surfing_ attack, where a third party intercepts your password by watching over your shoulder as you generate a fresh password.

Of course, this is only suitable if you are able to use the copy/paste feature to provide the password to the destination service.

!!!
Simply click/tap the (obfuscated) password in 2FAuth to copy it!
!!!

:icon-arrow-right: Check the [!badge size="l" icon="checkbox" text="Show generated one-time passwords as dot"] option in the 2FAuth's _Settings > Options_ section to enable obfuscation.
