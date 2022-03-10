+++
title = "GnuPG Cheatsheet"
date = "2022-03-04T16:00:00+01:00"
tags  = ['GnuPG', 'encryption', 'email', 'tech']
+++

![](https://upload.wikimedia.org/wikipedia/de/thumb/6/6b/GnuPG.svg/300px-GnuPG.svg.png)

That logo you have probably also noticed in the contact options on my landing page. That's simply because as uniform and decentralized email is, it has one major flaw: At default it's _not end-to-end encrypted_ and therefore completely unsecure.
GPG addresses exactly this problem by implementing the PGP (Pretty Good Privacy) protocol (pretty confusing naming scheme, I know ;) and offering asymetric encryption which is mostly used to ensure mail-conversation confidentiality and integrity.

More about it:
- https://www.gnupg.org/
- https://en.wikipedia.org/wiki/Public-key_cryptography

This article goes over the usage of GPG in order to teach how to use GPG for encrypting/decrypting and sining/verifying mails and documents.

### Install GnuPG
```
sudo pacman -Syu gnupg
```

### Configure directory
```
mkdir ~/.gnupg/
sudo chown -R "$(whoami):$(id -gn)" ~/.gnupg/
chmod -R 700 ~/.gnupg
```

### Configure keyserver

#### GnuPG
```
nano ~/.gnupg/gpg.conf
```
```
keyserver hkps://keys.openpgp.org
```
#### Seahorse (workaround; since adding keys doesn't work)
use dconf-editor: /desktop/gnome/crypto/pgp/keyservers > custom value (see Default) > `['hkps://keys.openpgp.org']`

## Generate key
```
cd ~/.gnupg
gpg --full-gen-key
```
```
# Please select what kind of key you want:
1
# What keysize do you want:
4096
Key is valid for:
1y
# Real name:
Nicko Hristov
# Email address:
mail@nickohristov.de
# Comment:
```

### Control keys
```
gpg --list-keys
gpg --list-secret-keys
```

## Configure keys

### Add more identities to the key
```
gpg --edit-key <key-id>
```
```
adduid
...
trust
5
save
```

### Edit keys
```
gpg --edit-key <key-id>
```
```
> passwd       # change the passphrase
> clean        # compact any user ID that is no longer usable
> revkey       # revoke a key
> addkey       # add a subkey to this key
> expire       # change the key expiration time
> adduid       # add additional names, comments, and email addresses
> addphoto     # add photo to key
> help         # show all commands
# save (after changes!)
```

## Revoke keys

### Generate revocation certificate
```
gpg --gen-revoke <key-id> --output revocation_certificate.asc --armor
chmod 700 ~/.gnupg/revocation_certificate.asc
```

### Merge key with revocation certificate
```
gpg --import revocation_certificate.asc
gpg --send-keys <key-id> --keyserver hkp://keyserver.ubuntu.com
```

## Exporting keys

### Export key to keyserver
```
gpg --send-keys <key-id> --keyserver hkp://keyserver.ubuntu.com
```

### Export public key as ASCII
```
gpg --export <key-id> --output public_key.asc --armor
```

### Export secret key as ASCII
```
gpg --export-secret-key <key-id> --output secret_key.asc --armor
```

## Import keys

### Import key
```
gpg --import <key-id>
```

### Import key from keyserver
```
gpg --search-keys <user-id>
gpg --recv-keys <key-id>
```

### Refresh local keys
```
gpg --refresh
```

## Verify and sign keys

### Sign key
```
gpg --edit-key <key-id>
```
```
sign
...
save
```

### Trust key
```
gpg --edit-key <key-id>
```
```
trust
1/2/3/4/5
save
```

## Encrypt and decrypt files manually

### Encrypt file
```
gpg --encrypt --recipient <recipient-public-key-id> <filename> --armor
```

### Decrypt file
```
gpg --decrypt <encryptedfilename> --output <newfilename>
```
