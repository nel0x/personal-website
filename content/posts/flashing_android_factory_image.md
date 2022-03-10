+++
title = "Flashing AOSP to Smartphones"
date = "2022-03-01T16:00:00+01:00"
tags  = ['android', 'customROM', 'smartphone']
+++

Android is a great open source smartphone operating system and because based on Linux, with almost unlimited customization possibilities. Unfortunately, vendors like to lock their ecosystem down and this is only true for "stock Android" or more precisely the AOSP (Android Open Source Platform) and nowadays _every single_ Android system shipped by popular manufacturers like OnePlus, Samsung and ZTE is bloated with tons of proprietary crapware and the even worse the Google services.

Some privacy-friendly/secure android Custom ROMs:
- https://e.foundation/ (All in one solution)
- https://iode.tech/en/ (All in one solution)
- https://grapheneos.org/ (very securtity-focused)
- https://calyxos.org/ (Trade-off between comfort and security)
- https://lineageos.org/ (multi-functional AOSP-Base)
- https://volla.online/de/ (VollaOS - beautiful but propriatary UI)

Howerver, actually most smartphones lock the bootloader of their devices in some kind of way, so that it's either completely impossible to install Custom ROMs or that doing so e.g. breaks the bootloader partition and leaves some irreversible mark.
So before trying to follow along this guide or even buying a new phone it's smart to make sure if the device supports such modifications.

But if it does, like (very ironically) the Google & OnePlus phones do, the following steps are required to unlock the bootloader, flash the android image and lock the bootloader again. This guide is written specificly for GrapheneOS and CalyxOS but should be roughly the way for flashing any android-based image; only slight changes may be required in the future, which always can be looked up at the corresponding projetct sites.

## Download the SDK Platform tools

https://developer.android.com/studio/releases/platform-tools

```
$ curl -O https://dl.google.com/android/repository/platform-tools_r30.0.4-linux.zip
$ echo '5be24ed897c7e061ba800bfa7b9ebb4b0f8958cc062f4b2202701e02f2725891  platform-tools_r30.0.4-linux.zip' | $ $ $ sha256sum -c
$ unzip platform-tools_r30.0.4-linux.zip
```

## Download the official android image

https://developers.google.com/android/images

## Unzip both
```
$ unzip *factory*.zip
$ unzip platform-tools*.zip
```

Then copy SDK-Tools Folder-content into Image folder and cd into the image folder.

## Correct fastboot Path

```
$ sudo nano flash-all.sh
```
Remove if-query Change every 'fastboot' to './fastboot'

## (For safety) Remove fastboot Distro-Programm
```
$ sudo pacman -R android-tools
```

## Then, on the phone:
- Enable OEM Unlocking (in the Developer Settings)
- Plugin the device directly to the computer
- Reboot into fastboot-mode by `Vol- + Power`

## List devices
```
$ sudo ./fastboot devices
```

## Unlock bootloader
```
$ sudo ./fastboot flashing unlock
```

## Flash Phone
```
$ sudo ./flash-all.sh
```

## First check if AOSP installed correctly (without yet setting it up!) 
## Then lock the bootloader again
```
$ sudo ./fastboot flashing lock
```
## Lastly: Restart, setup and enjoy your new OS! :)
