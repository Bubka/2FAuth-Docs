---
label: Welcome
---
# Welcome to 2FAuth Docs

> 2FAuth is a web based self-hosted alternative to One Time Passcode (OTP) generators like Google Authenticator, designed for both mobile and desktop.

<style>
    html.dark .light-screen,
    html:not(.dark) .dark-screen {
        display: none;
    }
    html.dark .dark-screen,
    html:not(.dark) .light-screen {
        display: block;
    }
</style>
:::dark-screen
![Screenshots of 2FAuth on mobile](/static/2fauth_screenshots_dark.png)
:::
:::light-screen
![Screenshots of 2FAuth on mobile](/static/2fauth_screenshots_light.png)
:::

## Why 2FAuth

Two-Factor Authentication has become very popular in recent years, resulting in more and more situations where we face a security code request and an increase in the number of accounts protected by this technology. In other words, 2FA is now inevitable and critical.

2FAuth's purpose is to simplify how you use and manage your 2FA with a clean and suitable interface, no matter what device you use. In front of your computer without your smartphone and dealing with a code request? No problemo, just open your 2FAuth instance in a browser tab and voilà!

Moreover, as an open source and self-hosted application, it lets you regain control over your personal security data, giving you privacy and the ability to back it up (Have you lost a smartphone with all your 2FA accounts inside Google Auth? I did... it really sucked)

## Features

#### :icon-ellipsis: Generate passwords

The main purpose of 2FAuth: Serve you some fresh TOTP/HOTP security codes aka One-Time Passwords.

#### :icon-device-desktop: Work anywhere

It's a Web App, it just works, whatever device you're on. You only need one device (not even yours) and an Internet connection.

#### :icon-apps: QR codes scan

Scan and decode QR codes to add a 2FA account in no time. Actually, it decodes any QR code, even non 2FA.

#### :icon-database: 2FA management

Manage your 2FA accounts, organize and classify them using Groups, edit & delete them. You can even manually add an account without scanning a QR code.

#### :icon-shield-check: Protect your data

2FAuth protects your data with Privacy, Self-hosting, Encryption, WebAuthn authentication, OTP obfuscation, and Auto lock.

#### :icon-people: Multi-user

Share your instance with your family, your friends. Everyone can have an account.

#### :icon-arrow-switch: Import / Export

Migrate from another 2FA app to 2FAuth or export your 2FA data in a breeze.

## REST API

2FAuth provides a REST API which lets you perform most of its functionalities from any external application. Have a look at the [API documentation](/api/) to find out how to use it.

## Demo website

Want to try 2FAuth? A demo is available at <https://demo.2fauth.app>

You can connect using the email address `demo@2fauth.app` and the password `demo`. The demo is reset every hour.

## Browser extensions

By design, 2FAuth is always at your fingertips: Through a pinned tab, shortcuts, or PWA installation. Browser extensions complement this feature by offering OTP generation directly from your browser toolbar.

Note: You must have a running instance of 2FAuth to use these extensions; they are not standalone.

<figure class="content-left">
    <a href="https://chromewebstore.google.com/detail/2fauth-beta/kokhpbhfeokchmbimdlaldcmlinjpipm" target="_blank">
        <img src="static/available_on_chrome_web_store_bordered.png" alt="Available on Chrome web store"/>
    </a>
</figure>
<figure class="content-left">
    <a href="https://addons.mozilla.org/fr/firefox/addon/2fauth-addon/" target="_blank">
        <img src="static/available_on_amo.webp" alt="Get the Firefox addon"/>
    </a>
</figure>
