---
label: Welcome
---
# Welcome to 2FAuth Docs

> 2FAuth is a web based self-hosted alternative to One Time Passcode (OTP) generators like Google Authenticator, designed for both mobile and desktop.

![Screenshots of 2FAuth on mobile](/static/2fauth_screenshots.png)

## Why 2FAuth

Two-Factor Authentication is become very popular over the past years resulting in more and more situations where we face a security code request and an increase of the number of accounts protected by this technology. In other words it is now inevitable and critical.

The purpose of 2FAuth is to ease you perform your 2FA authentication steps whatever the device you are using, with a clean and always suitable interface. In front of your computer without your smartphone and dealing with a code request? No problemo, just open your 2FAuth instance in a browser tab et voila!

Moreover, as an open source and self-hosted application, it lets you regain control over your personal security data, whether about their privacy or the ability to backup them (did you already loose a smartphone with all your 2FA accounts in and only in Google Auth? I did... it really sucks)

## Features

#### :icon-ellipsis: Generate password

The main purpose of 2FAuth: Serve you some fresh TOTP/HOTP security codes aka One-Time Passwords.

#### :icon-device-desktop: Work anywhere

It's a Web App, it just works, whatever is your device. You only need one (not even yours) and an Internet connection.

#### :icon-apps: QR codes scan

Scan and decode QR codes to add a 2FA account in no time. Actually it decodes any QR code, even non 2FA.

#### :icon-database: 2FA management

Manage your 2FA accounts, reorganize them and classify them using Groups, edit & delete them. You can even manually add an account without scanning a QR code.

## REST API

2FAuth provides a REST API which lets you perform most of its functionalities from any external application. Have a look to the [API documentation](/api/) to find out how to use it.

## Demo website

Want to try 2FAuth? A demo is available at <https://demo.2fauth.app>

You can connect using the email address `demo@2fauth.app` and the password `demo`. The demo is reset every hour.
