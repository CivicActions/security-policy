# Encryption

The following offers a brief overview of a few FOSS encryption tools that you can download and install to enhance the privacy of your interactions online. Besides the fact that you have a right to your privacy and are not committing any crimes, taking steps to ensure your privacy in a world that is increasingly fearful of others and creating new surveillance techniques is empowering at the least.

This document is light in content but enough links to additional documentation are included that an hour or two of your time should be sufficient to set up your GnuPG key pair and encrypted email communications.

You may want to start by reading [An Introduction to Public Key Cryptography and PGP](https://ssd.eff.org/en/module/introduction-public-key-cryptography-and-pgp), a [Surveillance Self-Defense](https://ssd.eff.org/en) tutorial from your friends at the [Electronic Frontier Foundation](https://www.eff.org/) [[Donate!](https://supporters.eff.org/donate/)]

Finally, keep an eye out for the dangerous [Burr-Feinstein Anti-Encryption Bill](https://www.techdirt.com/articles/20160527/08343534565/burr-feinstein-anti-encryption-bill-has-no-support-wont-be-moving-forward-anytime-soon.shtml) (also [here](https://www.wired.com/2016/04/senates-draft-encryption-bill-privacy-nightmare/), [here](https://www.engadget.com/2016/09/10/anti-encryption-bill-proposed-changes/) and [here](https://www.google.com/webhp#q=burr+feinstein+encryption+bill)) and be prepared to take action when it resurfaces to protect your rights to privacy and security.

Thank you!

Table of Contents
=================
* [Brief Introduction to GnuPG](#brief-introduction-to-gnupg)
  * [Creating your public/private key pair](#creating-your-publicprivate-key-pair)
  * [Encrypting a file so only your friend can read it](#encrypting-a-file-so-only-your-friend-can-read-it)
  * [Upload your private key to GPG key servers](#upload-your-private-key-to-gpg-key-servers)
  * [More GnuPG information](#more-gnupg-information)
* [Encrypting your email](#encrypting-your-email)
  * [Mailvelope (for Gmail in Chrome &amp; Firefox)](#mailvelope-for-gmail-in-chrome--firefox)
  * [GPG Suite (for Mac OSX Mail App)](#gpg-suite-for-mac-osx-mail-app)
  * [Enigmail (Mozilla Thunderbird)](#enigmail-mozilla-thunderbird)
  * [More Email References](#more-email-references)
* [Private Messaging and Calling](#private-messaging-and-calling)
* [Private Cash](#private-cash)

## Brief Introduction to GnuPG

The [GNU Privacy Guard](https://www.gnupg.org/) (GnuPG) is a complete and free implementation of the [OpenPGP standard](https://www.ietf.org/rfc/rfc4880.txt) (also known as PGP).

The current recommended version is GnuPG 2.x

* Mac OS X installers:
  * [GnuPG for OSX](https://sourceforge.net/p/gpgosx/docu/Download/) _(more recent)_
  * [GPG Suite](https://gpgtools.org/) _(integrates into Apple Mail)_
* GNU/Linux installers:
  * Ubuntu: `sudo apt-get install gnupg2`
  * Arch: `pacaur -S gnupg2`
  * Fedora/CentOS: `sudo yum|dnf install gnupg2`

### Creating your public/private key pair

After installing, you'll need to [generate a new key pair](https://www.apache.org/dev/openpgp.html#key-gen-generate-key):
```
$ gpg --gen-key
```

Choose:
- (1) RSA and RSA (default)
- keysize = 4096 bits
- 0 = key does not expire
- use your civicactions.com email address (you can add more email addresses later)
- use a strong pass phrase to protect your secret key

### Encrypting a file so only your friend can read it
First, you have to look up your friend's public key on a key server:
```
$ gpg --keyserver pool.sks-keyservers.net --search-keys 'fen labalme'
```

Once you have their key ID, you can run a command like:
```
$ gpg --keyserver pool.sks-keyservers.net --recv-key 446DB63655C12656
```

_(This will pull Fen's key and add it to your keyring.)_ Now you can encrypt a file so only your friend (in this case, Fen) can read it (the `--armor` argument creates an easy-to-cut-and-paste version on the encrypted document):
```
$ gpg --armor --output doc.asc --encrypt --recipient fen@civicactions.com doc
```

For quick help on the command line, do:
```
$ gpg -h
```

### Upload your private key to GPG key servers
You'll want to upload your public key to a keyserver so others can send you encrypted files. To [send your key to a keyserver](https://debian-administration.org/article/451/Submitting_your_GPG_key_to_a_keyserver), you need to know your key ID. You can print the information on all keys you have the private key for by running
```
$ gpg --list-secret-keys
```

This will generate output similar to the following:
```
$ gpg --list-secret-keys
/home/fen/.gnupg/pubring.kbx
----------------------------
sec   rsa4096/446DB63655C12656 2016-03-23 [SC]
uid                 [ultimate] Fen Labalme <fen@civicactions.com>
uid                 [ultimate] Fen Labalme <fen@openprivacy.org>
uid                 [ultimate] Fen Labalme <fen@comedia.com>
uid                 [ultimate] Fen Labalme <fen@alum.mit.edu>
uid                 [ultimate] Fen Labalme <fen.labalme@gmail.com>
ssb   rsa4096/F5176136558CF34A 2016-03-23 [E]
```

From this, you can see my primary key ID, `446DB63655C12656`. Now you can send your public key to the key servers with this command:
```
$ gpg --send-keys 446DB63655C12656
```

After some time for propagation (give it a few hours to a day) you can look up your public key by entering your email address or key id into a key serch engine like http://pgp.mit.edu/

### More GnuPG information
* [GnuPG home](https://www.gnupg.org/)
* [GnuPG Mini How-To](http://www.dewinter.com/gnupg_howto/english/GPGMiniHowto.html)
* [(Ubuntu) OpenPGP Key Signing Party](https://wiki.ubuntu.com/KeySigningParty)

You'll want to get your key signed and grow your [web of trust](https://en.wikipedia.org/wiki/Web_of_trust). And you'll want to integrate your key with your email client.

## Encrypting your email

You use email every day. Sending normal (un-encrypted) email is like sending a post card via the Post Office, as the mail will pass through many hands from sender to recipient and could be read by any of those people along the way. Largely because of the volume (and assuming that neither party is particularly famous) both the post card and the email likely passes along its way without anyone reading it. But they could.

In the case of post cards, generally it would be celebrity or a particularly interesting ing photo that might cause the card to be read. Email can be easily be scanned for specific content by sophisticated computer programs residing anywhere along the path. Types of content that your email may be reqularly scanned for might include:
* illegal music or movie downloads (in cooperation with the RIAA or MPAA)
* suspicious "terrorist" activities (in cooperation with the FBI or NSA)
* social security numbers, user names or passwords (by illegal black hat hackers)

Bottom line: while your post cards are likely not being read, your emails are at the least being scanned by automated sniffers. But it is possible to wrap your _post card_-like email in a secure envelope known as encryption. If you use strong encryption, it can actually be impossible for even the NSA to decrypt the ciphertext without your cooperation (or perhaps NSA-injected malware in your computer that steals your private key and passphrase).

There are many ways that you can integrate GPG with your email, several are described here:

### Mailvelope (for Gmail in Chrome & Firefox)
[Mailvelope](https://www.mailvelope.com/) integrates GPG with your Gmail using a Chrome or Firefox extension.

### GPG Suite (for Mac OSX Mail App)
See [GPGTools](https://gpgtools.org/) _(not yet fully integrated with Sierra)_

### Enigmail (Mozilla Thunderbird)
[Enigmail](https://www.enigmail.net/index.php/en/) works with Mozilla Thunderbird and GPG to deliver a seamless encrypted email experience.

### More Email References
* [Email Self-Defense](https://emailselfdefense.fsf.org/en/)
* [The Best Free Ways to Send Encrypted Email and Secure Messages](http://www.howtogeek.com/135638/the-best-free-ways-to-send-encrypted-email-and-secure-messages/)
* [Why No One Uses Encrypted Email Messages](http://www.howtogeek.com/187961/why-no-one-uses-encrypted-email-messages/)
* [Why You Should Encrypt Your Email](https://www.lifewire.com/you-should-encrypt-your-email-2486679)
* [How to Secure Your Google, Dropbox, and GitHub Accounts With a U2F Key](How to Secure Your Google, Dropbox, and GitHub Accounts With a U2F Key)

## Private Messaging and Calling

We recommend [Open Whisper Systems](https://whispersystems.org/). We like that their primary "forward secrecy" algorithm, along with the rest of their code, is GPL licensed on Github (https://github.com/whispersystems/).

## Private Cash

You've likely heard of the secure on-line money called [bitcoin](https://bitcoin.org/). The problem with bitcoin is that its transaction leger is public, to the seller, buyer and ammount of every transaction is published for anyone to see. Enter [Zcash](https://z.cash/) which adds extensions to bitcoin that offer total payment confidentiality (also on [Github](https://github.com/zcash/)).
