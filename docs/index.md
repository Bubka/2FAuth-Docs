# Introduction

Welcome to the 2FAuth documentation.

2FAuth is a web based self-hosted alternative to One Time Passcode (OTP) generators like Google Authenticator, designed for both mobile and desktop.

## Why 2FAuth

Two-Factor Authentication is become very popular over the past years resulting in more and more situations where we face a security code request and an increase of the number of accounts protected by this technology. In other words it is now inevitable and critical.

The purpose of 2FAuth is to ease you perform your 2FA authentication steps whatever the device you are using, with a clean and always suitable interface. In front of your computer without your smartphone and dealing with a code request? No problemo, just open your 2FAuth instance in a browser tab et voila!

Moreover, as an open source and self-hosted application, it lets you regain control over your personal security data, whether about their privacy or the ability to backup them (did you already loose a smartphone with all your 2FA accounts in and only in Google Auth? I did... it really sucks)

## Features

Here are the main features available in 2FAuth:

- Generate TOTP and HOTP security codes
- Scan and decode any QR code to add a 2FA account in no time
- Manage your 2FA accounts and organize them using Groups
- Manually add custom 2FA accounts for which no QR is available
- Edit accounts, even the ones imported from a QR code

### REST API

2FAuth provides a REST API which lets you perform most of the those features from any external client. Have a look to the [API documentation](/api/) to find out how to use it.

## Demo website

Want to try 2FAuth? A demo is available at <https://demo.2fauth.app>

You can connect using the email address `demo@2fauth.app` and the password `demo`. The demo is reset every hour.
