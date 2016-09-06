# Using your YubiKey

Notes on installing and setting up YubiKey Neo and YubiKey 4 on GNU/Linux and (soon) Mac OS X. Please help make this page more useful by adding links you found useful (describe exactly how they are useful) and and specific steps you used to install, configure, test your YubiKey.

Table of Contents
=================

* [Install packages:](#install-packages)
  * [Arch](#arch)
  * [Fedora](#fedora)
  * [Ubuntu, Xubuntu](#ubuntu-xubuntu)
  * [Mac OS X](#mac-os-x)
* [Enable OTP U2F CCID mode:](#enable-otpu2fccid-mode)
* [Setup PAM TFA](#setup-pam-tfa)
* [Setup screen lock on lid closed (Linux with X server)](#setup-screen-lock-on-lid-closed-linux-with-x-server)
* [Setup inactivity time based locking/suspend:](#setup-inactivity-time-based-lockingsuspend)
  * [Arch](#arch-1)
* [Setup Yubikey removal locking (GNU/Linux specific):](#setup-yubikey-removal-locking-gnulinux-specific)
* [Enable for Lastpass](#enable-for-lastpass)
* [Enable for Google](#enable-for-google)
* [Enable for AWS Root Account](#enable-for-aws-root-account)
* [Enable for AWS IAM Account](#enable-for-aws-iam-account)

## Install packages:

### Arch
_See also: https://wiki.archlinux.org/index.php/yubikey_
```
$ pacaur -S perl-net-ldap-server    # this is a prerequisite
$ pacaur -S yubikey-neo-manager-git yubico-pam
Plus your favorite screen locker (xautolock for timing and xsecurelock for locking below).
```

### Fedora
_See also: https://fedoraproject.org/wiki/Using_Yubikeys_with_Fedora_
```
dnf copr enable jjelen/yubikey-neo-manager 
dnf copr enable spartacus06/yubikey-utils 
dnf install yubikey-neo-manager yubioath-desktop yubikey-personalization-gui
```

### Ubuntu, Xubuntu
_See e.g.: https://developers.yubico.com/yubico-pam/Authentication_Using_Challenge-Response.html_

tbd...

### Mac OS X
_See also: https://www.yubico.com/why-yubico/for-individuals/computer-login/mac-os-login/_

tbd...

## Enable OTP+U2F+CCID mode:
This allows you to use your Yubikey with Google TFA (new fangled U2F), as well as Lastpass (which uses the OTP application).
```
$ neoman
# Enable OTP, U2F, CCID checkboxes, follow instructions to add and remove key.
```
Note: these checkboxes may already be checked on YubiKey 4 devices.

## Setup PAM TFA
PAM is the Pluggable Authentication Module used by Linux and Mac OS X to manage login authentication.

This will require the Yubikey (Two Factor Authentication) to be inserted to authenticate via PAM (login, sudo or screen unlock). Test this carefully in an alternate console session to ensure you don't lock yourself out!

See Yubico GitHub page for complete documentation (although this is for Ubuntu, which does some autoconfig on package install and is why I need to edit system-auth manually below).
```
​$ ykpersonalize -2 -ochal-resp -ochal-hmac -ohmac-lt64 -oserial-api-visible
$ ykpamcfg -2 -v
$ sudo vi /etc/pam.d/system-auth
# Add the following line at the top of the file:
auth      required  pam_yubico.so   mode=challenge-response
```

## Setup screen lock on lid closed (Linux with X server)
This uses xss-lock (the brains behind the venerable xscreensaver function) and i3lock as the screen locker, but you can substitute this with another locker such as xsecurelock. The basic idea is to start xss-lock when you start your window manager, passing it the name of the screen locker you want to use. So you could put this in your ~/.xinitrc file:
```
xss-lock -- i3lock -n -c 000000
```

Or if running the i3 windor manager, put this in your ~/.i3/config file:
```
exec --no-startup-id xss-lock -- i3lock -n -c 000000
```

## Setup inactivity time based locking/suspend:
This uses xsecurelock (recommended screen lock) together with xautolock (simple away command runner tool) to lock the screen after 10 minutes when away from home network. It also suspends after 30 mins, adds a hot corner to block locking (useful if watching a video, for example) and adds a notification (using dunst and notify-send) before locking. Note that pretty much all of these pieces are optional (you could use gnome-screensaver or xscreensaver for away detection for instance), but using xsecurelock for locking is strongly recommended since other lock screens have had vunerabilities.

Install packages as needed:

### Arch
```
$ pacaur -S xsecurelock-git xautolock dunst libnotify
```

Adapt the below script to lock your screen if you are away from home. Alternatively you can skip this and just replace the call to out-lock with a call to "/usr/bin/xsecurelock auth_pam_x11 saver_blank" in the xautolock or other away detector.

Assuming `~/bin/` is in your `$PATH`, create executable file `~/bin/out-lock`:
```
#!/bin/sh
# Not home (you will need to adjust to some reliable/secure test for your home network).
# In this case, an internal NAT addressable file hoe.txt has the given sha256sum value.
if ! curl -s 'http://192.168.1.99/home.txt' | sha256sum | grep 6094dd1d56b9d8638bc0e8e630683787151b81320d81568d97ec8daecb370bca > /dev/null; then
  # Not already locked.
  if ! pidof xsecurelock > /dev/null; then
    # Lock screen.
    /usr/bin/xsecurelock auth_pam_x11 saver_blank
  fi
fi
```

Next make sure dunst (if using for notifications) and xautolock (if using) are started on X login:

Adapt the following to start when your window manager starts, e.g., add to `~/.xinitrc`:
```
USER=`whoami`  # Better to replace '$USER' below with your user name
dunst &
xautolock -time 10 -corners -000 -locker '/home/$USER/bin/out-lock' -killtime 30 -killer 'systemctl suspend' -notify 30 -notifier "notify-send -- 'Locking screen in 30 seconds'" &
```

## Setup Yubikey removal locking (GNU/Linux specific):
This locks the laptop immediately when any Yubikey is removed. If you are not using xautolock as your "away detector", replace xautolock with a command to trigger your screen lock with the "away detector" that you do use. This is inspired by https://vtluug.org/wiki/Yubikey

Assuming `~/bin/` is in your `$PATH`, create executable file `~/bin/ykgone`:
```
#!/bin/bash
USER=`whoami`
getuser ()
    {
     export DISPLAY=":0"
     user=$(who | grep "$DISPLAY" | awk '{print $1}' | tail -n1)
     export XAUTHORITY=/home/$USER/.Xauthority
     eval $1=$USER
    }

getuser "$USER"
su $USER -c 'xautolock -locknow' &
```

Next, create (with sudo) a device notification file `/etc/udev/rules.d/90-yubikey.rules`:
```
ACTION=="remove", ATTRS{idVendor}=="1050", RUN+="/home/$USER/bin/ykgone"
```

## Enable for Lastpass

This requires a Yubikey OTP (cover the button for ~1s) on Desktop and a NFC contact on mobile to unlock Lastpass.
- Vault -> Settings -> Multifactor options
- Set "Mobile" to disallowed, if your phone supports NFC (touch against Neo to unlock on mobile).

## Enable for Google
For each Google account you have:
- Visit https://accounts.google.com/b/0/SmsAuthSettings#devices
- Enable TFA, and complete the phone verification process (phone will act as backup TFA).
- Click on "Security Keys" and follow instructions to add Yubikey.
- Return to the main page and add a second phone and/or print backup codes.
- As long as you have a backup, you can also install the Yubikey Authenticator app, and configure your account to use that for the backup TFA instead of SMS/phone - this is the same as the Google Authenticator app, except that it stores the credentials on your Yubikey instead of the phone.
- If you have funky devices/apps that don't support TFA, you can set an application specific password using that tab. This includes sending E-mail from your personal Gmail account using your civicactions.com IMAP, for instance.

## Enable for AWS Root Account
For each AWS account you have:
- Visit https://console.aws.amazon.com/iam/home?region=us-east-1#security_credential
- Under MFA, add a Virtual MFA device.
- Use Yubikey Authenticator app to scan the QR code, and enter the reponse code, then close and reopen the app and enter the second response code.

## Enable for AWS IAM Account
- Visit https://console.aws.amazon.com/iam/home?region=us-east-1#users
- Choose your user name
- Click on Manage your MFA device
- Use Google Authenticator app to scan the QR code, and enter the reponse code
- then close and reopen the app and enter the second response code.
- _using YubiKey untested - don't have Yubikey Authenticator set up_
