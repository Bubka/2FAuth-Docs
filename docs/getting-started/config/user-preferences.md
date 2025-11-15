---

---
# User preferences

## Purpose

User preferences are settings that any user can change to customize the behavior or appearance of 2FAuth. They can be set by the user from the _Settings > Options_ section of 2FAuth. As the name implies, when a user edits a preference, it only affects their own use, not that of other users.

2FAuth comes with a set of default values for user preferences. These defaults are defined to provide a good starting experience for every user, but as an administrator, you may want to control them for a consistent, streamlined, or corporate user experience. 2FAuth supports 2 ways to accomplish this:

__Custom defaults__
:   2FAuth defaults are overridden with your default values.

    When a preference's default is customized, the custom value is applied to all users who didn't already personalized the preference themselves from the _Settings > Options_ page of 2FAuth. The preference can still be modified by user.

__Preference locking__
:   Preferences are locked for change to user.

    Regardless of the value of the preference, no user can edit a locked preference from the _Settings > Options_ section of 2FAuth. The applied value is the last one, according to this priority order:

    1. By the user
    2. The custom default (if defined)
    3. The 2FAuth default

The two features can be combined for a complete control over the preferences:

__Custom defaults__ + __Preference locking__
:   Your custom defaults are enforced for all users.

    Regardless of the value of the preference, no user can edit a locked preference from the _Settings > Options_ section of 2FAuth. The applied value is yours, for everybody.

!!!info
Custom defaults and preference locking are done by setting up dedicated environment variables.  
Read the [Environment variables](/getting-started/config/env-vars/#how-to) page to learn how to define them.
!!!

## How to

Configuration is done per preference. To customize or lock a user preference, add a new environment variable using the following format:

```env
[PREFIX]__[NAME_OF_THE_USER_PREFERENCE]=[value]
```

Where `[PREFIX]` is to be replaced with the expected behavior, `[NAME_OF_THE_USER_PREFERENCE]` with the name of the user preference you want to impact and `[value]` with the required value. (see [Available preferences](#available-preferences) to discover the available preferences and their supported values)

!!!warning
Note the separator between the prefix and the var name, it's a double underscore: `__`
!!!

The 2 possible values to replace `[PREFIX]` are `USERPREF_DEFAULT__` for setting a custom default value and `USERPREF_LOCKED__` for locking. You have to set an environment variable for each behavior you want to apply.

A locking environment variable is only relevant if you want to lock a preference. If you don't need to, just don't declare such a variable, or delete the existing one.

### Example

Say you want to hide one-time password's icons in 2FAuth. The corresponding user preference is named [`SHOW_ACCOUNTS_ICONS`](#show_accounts_icons).  
Here are the environment variables to set, depending on your needs:

||| For a custom default only

```env
USERPREF_DEFAULT__SHOW_ACCOUNTS_ICONS=false
```

||| For pref locking only

```env
USERPREF_LOCKED__SHOW_ACCOUNTS_ICONS=true
```

||| For a locked & enforced pref

```env
USERPREF_DEFAULT__SHOW_ACCOUNTS_ICONS=false
USERPREF_LOCKED__SHOW_ACCOUNTS_ICONS=true
```

|||

Here is another example to control the theme of 2FAuth, given that the 2FAuth default for this preference is `system` (See [`THEME`](#theme))

||| For a custom default only

```env
USERPREF_DEFAULT__THEME=dark
```

||| For pref locking only

```env
USERPREF_LOCKED__THEME=true
```

||| For a locked & enforced pref

```env
USERPREF_DEFAULT__THEME=dark
USERPREF_LOCKED__THEME=true
```

|||

### Impact on running app

Because of the way 2FAuth is built, adding new environment variables to enforce user preferences may not have an immediate effect if your 2FAuth instance is running.

You must rebuild the configuration cache of the app for the new variables to be loaded or restart your container if you use one. See the [Environment variables](/getting-started/config/env-vars/#how-to) section for details.

Also, logged-in users won't see any changes until they reconnect or visit the _Settings > Options_ page of 2FAuth.

## Available preferences

:::is-half-width

- [AUTO_CLOSE_TIMEOUT](#auto_close_timeout)
- [AUTO_SAVE_QRCODED_ACCOUNT](#auto_save_qrcoded_account)
- [CLEAR_SEARCH_ON_COPY](#clear_search_on_copy)
- [CLOSE_OTP_ON_COPY](#close_otp_on_copy)
- [COPY_OTP_ON_DISPLAY](#copy_otp_on_display)
- [DEFAULT_CAPTURE_MODE](#default_capture_mode)
- [DISPLAY_MODE](#display_mode)
- [FORMAT_PASSWORD](#format_password)
- [FORMAT_PASSWORD_BY](#format_password_by)
- [GET_OFFICIAL_ICONS](#get_official_icons)
- [GET_OTP_ON_REQUEST](#get_otp_on_request)
- [ICON_COLLECTION](#icon_collection)
- [ICON_PACK](#icon_pack)
- [ICON_SOURCE](#icon_source)
- [ICON_VARIANT](#icon_variant)
- [ICON_VARIANT_STRICT_FETCH](#icon_variant_strict_fetch)

:::

:::is-half-width

- [KICK_USER_AFTER](#kick_user_after)
- [LANG](#lang)
- [NOTIFY_ON_FAILED_LOGIN](#notify_on_failed_login)
- [NOTIFY_ON_NEW_AUTH_DEVICE](#notify_on_new_auth_device)
- [REMEMBER_ACTIVE_GROUP](#remember_active_group)
- [REVEAL_DOTTED_OTP](#reveal_dotted_otp)
- [SHOW_ACCOUNTS_ICONS](#show_accounts_icons)
- [SHOW_EMAIL_IN_FOOTER](#show_email_in_footer)
- [SHOW_NEXT_OTP](#show_next_otp)
- [SHOW_OTP_AS_DOT](#show_otp_as_dot)
- [SORT_CASE_SENSITIVE](#sort_case_sensitive)
- [THEME](#theme)
- [TIMEZONE](#timezone)
- [USE_BASIC_QRCODE_READER](#use_basic_qrcode_reader)
- [USE_DIRECT_CAPTURE](#use_direct_capture)
- [VIEW_DEFAULT_GROUP_ON_COPY](#view_default_group_on_copy)

:::

<div style="clear: both;"></div>

:::has-following-h3-more-spaced
:::

### AUTO_CLOSE_TIMEOUT

[!badge variant="info" text="number"] [!badge variant="info" text="since v5.3"]

:::env-var-dl-wrapper

Description
:   Time before the on-screen one-time password is automatically closed.

    Only relevant when [GET_OTP_ON_REQUEST](#get_otp_on_request) is set to `true`.

Default value
:   `2`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`0`
:   Disables the auto-close feature, the popup containing the OTP will never be closed

`1` , `2` , `5`
:   Duration (in minutes) before the auto-close is triggered

:::

### AUTO_SAVE_QRCODED_ACCOUNT

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.2"]

:::env-var-dl-wrapper

Description
:   Enables automatic registering of a 2FA account after scanning or uploading a QR code

Default value
:   `false`

:::

### CLEAR_SEARCH_ON_COPY

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.1"]

:::env-var-dl-wrapper

Description
:   Clears the search field when a click/tap is done on a one-time password to copy it

Default value
:   `false`

:::

### CLOSE_OTP_ON_COPY

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v1.1"]

:::env-var-dl-wrapper

Description
:   Closes the on-screen one-time password after a click/tap is done on it to copy it.

    Only relevant when [GET_OTP_ON_REQUEST](#get_otp_on_request) is set to `true`.

Default value
:   `false`

:::

### COPY_OTP_ON_DISPLAY

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v3.4"]

:::env-var-dl-wrapper

Description
:   Triggers OTP copy to clipboard when the OTP is displayed.

    Only relevant when [GET_OTP_ON_REQUEST](#get_otp_on_request) is set to `true`.

Default value
:   `false`

:::

### DEFAULT_CAPTURE_MODE

[!badge variant="info" text="string"] [!badge variant="info" text="since v2.0"]

:::env-var-dl-wrapper

Description
:   Default input mode to use when Direct capture is enabled

    Only relevant when [USE_DIRECT_CAPTURE](#use_direct_capture) is set to `true`.

Default value
:   `livescan`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`livescan`
:   Launches the device camera for flashing QR codes

`upload`
:   Prompt to upload a file from the device

`advancedForm`
:   Opens the advanced form
:::

### DISPLAY_MODE

[!badge variant="info" text="string"] [!badge variant="info" text="since v1.2"]

:::env-var-dl-wrapper

Description
:   The 2FA accounts display mode

Default value
:   `list`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`list`
:   Show 2FA accounts as a list

`grid`
:   Show 2FA accounts as a grid

:::

### FORMAT_PASSWORD

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v4.0"]

:::env-var-dl-wrapper

Description
:   Applies a digit formatting to one-time password display.

    Additional preferences have to be set to refine this behavior:

    - [FORMAT_PASSWORD_BY](#format_password_by)

Default value
:   `true`

:::

### FORMAT_PASSWORD_BY

[!badge variant="info" text="number"] [!badge variant="info" text="since v4.0"]

:::env-var-dl-wrapper

Description
:   Formatting pattern applied to one-time password display.

    Only relevant when [FORMAT_PASSWORD](#format_password) is set to `true`.

Default value
:   `0.5`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`0.5`
:   Groups password digits by half, like `#### ####`

`2`
:   Groups password digits by pair, like `## ## ##`

`3`
:   Groups password digits by trio, like `### ###`

:::

### GET_OFFICIAL_ICONS

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v3.3"]

:::env-var-dl-wrapper

Description
:   Allows 2FAuth to fetch the official icon of the 2FA provider when a new 2FA account is registered.

    Additional preferences have to be set to refine this preference:

    - [ICON_SOURCE](#icon_source)

Default value
:   `true`

:::

### GET_OTP_ON_REQUEST

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v4.1"]

:::env-var-dl-wrapper

Description
:   Generates and displays one-time passwords in a dedicated popup, only when the user requests them by clicking on 2FA account titles.

    When disabled, OTPs are always visible on the main screen, no action is required to generate or rotate them.

    Additional preferences may be set to refine this behavior:

    - [CLOSE_OTP_ON_COPY](#close_otp_on_copy)
    - [AUTO_CLOSE_TIMEOUT](#auto_close_timeout)
    - [COPY_OTP_ON_DISPLAY](#copy_otp_on_display)

Default value
:   `true`

:::

### ICON_COLLECTION

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.6"]

:::env-var-dl-wrapper

Description
:   The online icon library queried when 2FAuth needs to fetch an icon.

    Only relevant when [ICON_SOURCE](#icon_source) is set to `logolib`.

Default value
:   `selfh`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`selfh`
:   The [selfh.st](https://selfh.st/icons/) icon library

`dashboardicons`
:   The [dashboardicons.com](https://dashboardicons.com/) icon library

`tfa`
:   The [2fa.directory](https://2fa.directory/) icon library

:::

### ICON_PACK

[!badge variant="info" text="string"] [!badge variant="info" text="since v6.0"]

:::env-var-dl-wrapper

Description
:   The name of the icon pack queried when 2FAuth needs to fetch an icon.

    An icon pack is merely a collection of image files stored in a directory on the server, in the `[2fauth_install_dir]/storage/app/iconPacks/` location. The name of the icon pack match the name of the directory.

    If the icon pack is stored in a subdirectory, e.g. `[2fauth_install_dir]/storage/app/iconPacks/mysubdir/myiconpack/`, the preference must be set using the subdirectory and pack directory names combined, so `mysubdir/myiconpack`.

    Only relevant when [ICON_SOURCE](#icon_source) is set to `iconpack`.

Default value
:   `null`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

Any icon pack name uploaded to the server
:   e.g. `myiconpack` or `mysubdir/myiconpack`

### ICON_SOURCE

[!badge variant="info" text="string"] [!badge variant="info" text="since v6.0"]

:::env-var-dl-wrapper

Description
:   The location where 2FAuth offers to fetch icons

Default value
:   `logolib`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`logolib`
:   Icons are fetched from online icon libraries.

    Additional preferences have to be set to refine this option:

    - [ICON_COLLECTION](#icon_collection)
    - [ICON_VARIANT](#icon_variant)
    - [ICON_VARIANT_STRICT_FETCH](#icon_variant_strict_fetch)

`iconpack`
:   Icons are fetched from icon packs uploaded to the server by the 2FAuth administrator.

    Additional preferences have to be set to refine this option:

    - [ICON_PACK](#icon_pack)

:::

### ICON_VARIANT

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.6"]

:::env-var-dl-wrapper

Description
:   Some icon libraries provide icons in different variations to best suit dark or light user interfaces. Use this preference to indicate 2FAuth the flavor that must be requested first. The regular variant will be fetched automatically by 2FAuth if the specified one is not available.

    Only relevant when [ICON_SOURCE](#icon_source) is not set to `logolib`.

Default value
:   `regular`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`regular`
:   The neutral variant

`light`
:   The light variant

`dark`
:   The dark variant

:::

### ICON_VARIANT_STRICT_FETCH

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.6"]

:::env-var-dl-wrapper

Description
:   Narrow the fetch to the specified icon variant. If the variant is missing, 2FAuth will not try to fallback to the regular variant.

    Only relevant when [ICON_SOURCE](#icon_source) is set to `logolib` and [ICON_VARIANT](#icon_variant) is not set to `regular`.

Default value
:   `false`

:::

### KICK_USER_AFTER

[!badge variant="info" text="number"] [!badge variant="info" text="since v1.3"]

:::env-var-dl-wrapper

Description
:   Action or inactivity time span that triggers the automatic disconnection of the user, aka Auto-lock

Default value
:   `15`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`0`
:   Disables the auto-lock feature, the user will never be logged out

`-1`
:   Triggers user logout when a click/tap is done on a one-time password to copy it

`1` , `5` , `10` , `15` , `30` , `60` , `1440`
:   Time (in minutes) before the user is automatically disconnected

:::

### LANG

[!badge variant="info" text="string"] [!badge variant="info" text="since v4.0"]

:::env-var-dl-wrapper

Description
:   The language used to translate the 2FAuth user interface

Default value
:   `browser`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`browser`
:   Uses the preferred language setting of the browser used to visit 2FAuth.

Any supported language code
:   The language code of any supported translation, for example `fr`, `en` or `ja`.  
    See the <a href="https://crowdin.com/project/2fauth" target="_blank">Crowdin project page</a> for the full list.

:::

### NOTIFY_ON_FAILED_LOGIN

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.2"]

:::env-var-dl-wrapper

Description
:   Enables email notification of failed login attempts

Default value
:   `false`

:::

### NOTIFY_ON_NEW_AUTH_DEVICE

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.2"]

:::env-var-dl-wrapper

Description
:   Enables email notification of successful logon from a new device

Default value
:   `false`

:::

### REMEMBER_ACTIVE_GROUP

[!badge variant="info" text="boolean"]

:::env-var-dl-wrapper

Description
:   Saves the last group filter applied and restores it on your next visit

Default value
:   `true`

:::

### REVEAL_DOTTED_OTP

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.0"]

:::env-var-dl-wrapper

Description
:   Displays a button to make the OTP readable while it is obfuscated.

    Only relevant when [SHOW_OTP_AS_DOT](#show_otp_as_dot) is set to `true`.

Default value
:   `false`

:::

### SHOW_ACCOUNTS_ICONS

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v1.3"]

:::env-var-dl-wrapper

Description
:   Show/Hide the icons that depict 2FA accounts

Default value
:   `true`

:::

### SHOW_EMAIL_IN_FOOTER

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.4"]

:::env-var-dl-wrapper

Description
:   Switch between the old footer layout and the new one that displays the email address of the currently logged in user

Default value
:   `true`

:::

### SHOW_NEXT_OTP

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.5"]

:::env-var-dl-wrapper

Description
:   Previews the next OTP near the current valid OTP

Default value
:   `true`

:::

### SHOW_OTP_AS_DOT

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v1.1"]

:::env-var-dl-wrapper

Description
:   Obfuscates OTPs by replacing digits with dots.

    Additional preferences may be set to refine this behavior:

    - [REVEAL_DOTTED_OTP](#reveal_dotted_otp)

Default value
:   `false`

:::

### SORT_CASE_SENSITIVE

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v3.3"]

:::env-var-dl-wrapper

Description
:   Applies case-sensitive sorting when reordering 2FA accounts

Default value
:   `false`

:::

### THEME

[!badge variant="info" text="string"] [!badge variant="info" text="since v4.0"]

:::env-var-dl-wrapper

Description
:   The theme applied to the app pages

Default value
:   `system`

:::

<span class="fw-500">Accepted values</span>

:::sub-dl-wrapper

`system`
:   Uses the website appareance setting of the browser used to visit 2FAuth

`light`
:   Uses a light theme for backgrounds and page content

`dark`
:   Uses a dark theme for backgrounds and page content

:::

### TIMEZONE

[!badge variant="info" text="string"] [!badge variant="info" text="since v5.2"]

:::env-var-dl-wrapper

Description
:   The time zone applied to all dates and times displayed in the 2FAuth user interface

Default value
:   `UTC`

Accepted values
:   Any timezone identifier, for example `Africa/Asmara`.  
    See <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones" target="_blank">List of TZ database time zones</a>

:::

### USE_BASIC_QRCODE_READER

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v1.2"]

:::env-var-dl-wrapper

Description
:   Switches to an alternative QR code reader

Default value
:   `false`

:::

### USE_DIRECT_CAPTURE

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v2.0"]

:::env-var-dl-wrapper

Description
:   Skips the input mode selection screen.

    Additional preferences have to be set to refine this behavior:

    - [DEFAULT_CAPTURE_MODE](#default_capture_mode)

Default value
:   `false`

:::

### VIEW_DEFAULT_GROUP_ON_COPY

[!badge variant="info" text="boolean"] [!badge variant="info" text="since v5.1"]

:::env-var-dl-wrapper

Description
:   Always return to the default group filter when a click/tap is done on a one-time password to copy it

Default value
:   `false`

:::
