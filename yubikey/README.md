# Using your YubiKey

Notes on installing and setting up YubiKey Neo and YubiKey 4 for various platforms and applications.

Table of Contents
=================

* [Basic Setup (do this first)](#basic-setup-do-this-first)
* [Enable YubiKey TFA for applications](#enable-yubikey-tfa-for-applications)
  * [Lastpass](#lastpass)
  * [Google](#google)
  * [AWS Root Account](#aws-root-account)
  * [AWS IAM Account](#aws-iam-account)

## Basic Setup (do this first)
Before you can have your YubiKey act as a second (hardware) authentication token for your system, you need to install and configure some software that connects your YubiKey to the screen locking software on your computer. Instructions have been separated into two files:
- [GNU/Linux specific instructions](linux.md)
- [Mac OS X specific instructions](macosx.md)

## Enable YubiKey TFA for applications

### Lastpass
This requires a Yubikey OTP (cover the button for approximately one second) on laptop/desktop and a NFC contact on mobile to unlock LastPass.
- Vault -> Settings -> Multifactor options
- Set "Mobile" to disallowed, if your phone supports NFC (touch against Neo to unlock on mobile).

### Google
For each Google account you have:
- Visit https://accounts.google.com/b/0/SmsAuthSettings#devices
- Enable TFA, and complete the phone verification process (phone will act as backup TFA).
- Click on "Security Keys" and follow instructions to add Yubikey.
- Return to the main page and add a second phone and/or print backup codes.
- As long as you have a backup, you can also install the Yubikey Authenticator app, and configure your account to use that for the backup TFA instead of SMS/phone - this is the same as the Google Authenticator app, except that it stores the credentials on your Yubikey instead of the phone.
- If you have funky devices/apps that don't support TFA, you can set an application specific password using that tab. This includes sending E-mail from your personal Gmail account using your civicactions.com IMAP, for instance.

### AWS Root Account
For each AWS account you have:
- Visit https://console.aws.amazon.com/iam/home?region=us-east-1#security_credential
- Under MFA, add a Virtual MFA device.
- Use Yubikey Authenticator app to scan the QR code, and enter the reponse code, then close and reopen the app and enter the second response code.

### AWS IAM Account
- Visit https://console.aws.amazon.com/iam/home?region=us-east-1#users
- Choose your user name
- Click on Manage your MFA device
- Use Google Authenticator app to scan the QR code, and enter the reponse code
- then close and reopen the app and enter the second response code.
- _using YubiKey untested - don't have Yubikey Authenticator set up_
