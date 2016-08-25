# Security Awareness and Tools

This is an appendix to the [CivicActions Security Policy](/README.md) containing details that can and should be updated regularly as new technologies or patterns of use develop, with a goal of providing team members the best tools with which to maintain the security of company confidential and personal information.

_This is currently - and probably always will be - a work in progress. Pull Requests welcome._

## Table of Contents
1.  [Password Management Tools](#password-management-tools)
    - [Use a Password Manager](#use-a-password-manager)
	- [LastPass](#lastpass)
2.  [Use Two Factor (or 2-Step) Authentication (TFA, 2FA)](#use-two-factor-or-2-step-authentication-tfa-2fa)
    - [Two-Factor Authenticators](#two-factor-authenticators)
    - [Partial List of TFA Services](#partial-list-of-tfa-services)
    - [TFA Backup Codes](#tfa-backup-codes)
3.  [IT: Sharing Service Accounts](#it-sharing-service-accounts)
4.  [Phishing and Social Engineering](#phishing-and-social-engineering)
5.  [Keep Your Systems Up-to-date](#keep-your-systems-up-to-date)
6.  [Backups](#backups)
7.  [Securely Delete Files and Wipe Disks](#securely-delete-files-and-wipe-disks)
8.  [Drupal Best Practices and Writing Secure Code](#drupal-best-practices-and-writing-secure-code)

## Password Management Tools

### Use a Password Manager
A password manager will enable you to have unique, strong passwords for every service that you log into. Good password managers will generate new passwords for you, auto-fill web forms, allow extra protection for high-security accounts (like banking), and more. Choose a password manager that encrypts locally (in your browser, so you don’t have to trust the provider to keep their data safe) and that has iPhone and Android apps that will auto-sync with the manager. At CivicActions, we currently recommend LastPass as it is the most full-featured, but we are keeping a close eye on the FOSS KeePass and Password Safe solutions.

### LastPass
- The [LastPass](https://lastpass.com/) password generator can easily create and maintain hundreds of different 16 character (or more!) passwords. And LastPass has free iPhone and Android apps.
- LastPass is used by the CivicActions System Admins and Infrastructure Support Team.
- We recommend LastPass premium but do not require it. A premium account will enable unlimited sync across your devices and more robust two-factor authentication.
- It is fine (and perhaps preferable, because your browser can only use one LastPass account at a time) to use a personal email address to create your LastPass account. 

## Use Two Factor (or 2-Step) Authentication (TFA, 2FA)
Two factor authentication includes something you know (e.g. your memorized password) and something you have (e.g. your smartphone or a YubiKey) and can greatly increase the security of your systems. CivicActions recommends you use Two Factor Authentication for services that support it.

For example, as your password manager grows to have more passwords in it - not only CivicActions’ systems and clients but also your personal bank accounts, credit cards, school records, etc. - it becomes increasingly important to have it protected by more than just a password.

CivicActions requires that its employees and contractors that are given access to the CivicActions Google Apps - that include GMail, Hangouts and Google Docs access - use Two Factor Authentication on their CivicActions Google Account.

### Two-Factor Authenticators
There are many hardware and software tools for creating secure “one time passwords” (OTP). Two that we frequently use internally are described below.

*Google Authenticator*
 - For installation instructions, see https://support.google.com/accounts/answer/1066447
 - This page also has instructions for setting up 2-Step Verification for multiple Google accounts.

*YubiKey*
- See YubiKey documentation on how to use TFA with: [GMail](https://www.yubico.com/why-yubico/for-individuals/gmail-for-individuals/), [LastPass](https://www.yubico.com/why-yubico/for-individuals/password-managers/lastpass/), and [GitHub](https://www.yubico.com/why-yubico/for-individuals/github/)

### Partial List of TFA Services

- LastPass: [Multifactor Authentication Options](https://helpdesk.lastpass.com/multifactor-authentication-options/)
- Google: [2 Step Verification](https://support.google.com/accounts/topic/28786?hl=en&ref_topic=3382253)
- GitHub (especially for your [CivicAction account](https://github.com/CivicActions)): [Securing your account with two-factor authentication (2FA)](https://help.github.com/articles/securing-your-account-with-two-factor-authentication-2fa/)
- GitLab: See [your profile](https://git.civicactions.net/profile/account)
- Slack: [Enabling two-factor authentication](https://get.slack.help/hc/en-us/articles/204509068-Enabling-two-factor-authentication#enablingtwofactor-authentication)

### TFA Backup Codes

As a final step when setting up Two Factor Authentication with most services (Google, LastBass, Github, etc.) you will be offered a chance to download and/or print a set of backup codes. These are worth downloading and printing (yes - on paper) and storing in a safe place, because if you lose your phone or YubiKey it can be difficult to regain access to your account unless you have these codes available. There are sometimes other options available, too, like SMS text message, but these other methods can be less secure.

## IT: Sharing Service Accounts

- If a service allows individual accounts, use only individual accounts and not shared credentials.
- Prefer services that allow individual accounts, services that allow TFA and secure password policies.
- If a service only allows a single account, have a shared LastPass master account that ideally only 2-3 trusted people have access to. From there share passwords out on an "as needed" basis only, including to individual day-to-day Lastpass accounts for the 2-3 trusted people.
- If the LastPass master account is a paid account it also allows sharing credentials in a way that makes the password harder for the person who you shared it with to recover/view/share (but still allow them to log in with it).
- Shared account passwords should rotate to ensure that only those users needing access continue to have access, revoking individual accounts particularly when people leave.

## Phishing and Social Engineering

Social engineering is the most common attack vector used to compromise computer systems. Social engineering relies heavily on human interaction and often involves tricking people into breaking normal security procedures. The following is a brief reminder of some of the methods used, but is in no way complete.

- Phishing
    - Don’t click on links, hover and check the URLs
    - Don’t open attachments (unless you really trust the sender)
    - If in doubt, ask an IT member (e.g. via Slack)
	
- Windows Technical Support
    - "Windows Technical Support has noticed that you have viruses or other malware on your computer..."
	
- Baiting
    - Seemingly innocent (or interesting) abandoned USB, CD, DVD media with autorun
	
- Public WiFi (e.g., coffee shop, airport, library, ...)
    - Turn off sharing
    - Don’t automatically connect to unknown WiFi hotspots
    - Confirm the network name - know the name of your hotspot!
    - Turn on your local firewall
    - As usual, never enter your name or password information when on an insecure connection

## Keep Your Systems Up-to-date

One of the best ways to protect yourself from being hacked (other than via a social engineering pathway) is to keep your software on your computers and phones up-to-date. Sometimes you may reasonably want to wait for a .1 or .2 release before updating, but don’t get far behind.

## Backups

With more work captured in the cloud by Slack, Gmail, Google Drive, GitHub, etc. there is less that needs to be backed up. But you won't know what you'll miss until your system doesn't boot up because of an unrecoverable hard drive (or SSD) error. At the least, back up your security keys and personal preferences directories, such as (examples in GNU/Linux):

- `~/.ssh/`
- `~/.gnupg/`
- `~/.config`

Consider committing your personalization files (like `~/.bashrc`) into a Git repository. Just make sure that you do *not* commit any files that may contain private keys or passwords.

Also, take care not to backup any company or client sensitive files or data as it is important that that be completely deleted from your machine(s) when you stop working for that client.

Finally, if you use any backup mechanism more complicated than simply copying the files to another medium, ensure that you know how to restore the files, too, as backups are worthless if you can't retrieve them.

## Securely Delete Files and Wipe Disks

When you delete a file, it doesn't actually go away. Usually, all that occurs is the file name and a pointer to its bits are removed from a directory listing. With the proper tools, deleted files can be recovered. For this reason, it's important that old disks be securely wiped before being given or thrown away. Some pointers are given below:

GNU/Linux:
- http://askubuntu.com/questions/57572/how-to-delete-files-in-secure-manner
- https://ssd.eff.org/en/module/how-delete-your-data-securely-linux

Mac OS X:
- https://support.apple.com/kb/PH18638 (Yosemite)
- https://www.intego.com/mac-security-blog/how-to-securely-empty-trash-in-os-x-el-capitan/ (El Capitan)

## Drupal Best Practices and Writing Secure Code

CivicActions Drupal Developers are expected to know, understand and integrate into their work:
- [Drupal coding standards and best practices](https://www.drupal.org/developing/best-practices)
- [How to write secure code in Drupal](https://www.drupal.org/writing-secure-code)
