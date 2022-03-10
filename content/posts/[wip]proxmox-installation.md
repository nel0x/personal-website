+++
title = "Manually install encrypted Proxmox"
date = "2099-02-10T18:00:00+01:00"
tags  = ['debian', 'linux', 'proxmox', 'homelab']
+++

# Import OVA-Image in Proxmox
https://ryanburnette.com/blog/proxmox-import-ova/

# Export OVA-Image from Proxmox
https://ryan.himmelwright.net/post/exporting-proxmox-vms/

# Install encrypted Proxmox manually on Debian
https://www.sidorenko.io/post/2019/09/full-encrypted-proxmox-installation/

- dropbear cryptsetup guide
https://www.dwarmstrong.org/remote-unlock-dropbear/
`ssh -i .ssh/owlnest-env_rsa root@10.0.1.10`

- official Proxmox instructions
https://pve.proxmox.com/pve-docs/pve-admin-guide.html#sysadmin_package_repositories
https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_11_Bullseye

# For repos see Pictures in `/static/proxmox-installation-archive`

# Storage
## LUKS on LVM (recommended with multiple drives, for entering crypt-password only once)
```
root@pve:~# lsblk
NAME                    MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                       8:0    0  3.6T  0 disk  
├─sda1                    8:1    0  953M  0 part  /boot/efi
├─sda2                    8:2    0  954M  0 part  /boot
└─sda3                    8:3    0  3.6T  0 part  
  └─vg0-lv_root         253:0    0  5.5T  0 lvm   
    └─vg0-lv_root_crypt 253:1    0  5.5T  0 crypt /
sdb                       8:16   0 16.4T  0 disk  
└─sdb1                    8:17   0 16.4T  0 part  
sdc                       8:32   0  1.8T  0 disk  
└─sdc1                    8:33   0  1.8T  0 part  
  └─vg0-lv_root         253:0    0  5.5T  0 lvm   
    └─vg0-lv_root_crypt 253:1    0  5.5T  0 crypt /

root@pve:~# cat /etc/fstab
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/vg0-lv_root_crypt /               ext4    errors=remount-ro 0       1
# /boot was on /dev/sda2 during installation
UUID=f8a47d20-aef0-431e-963a-eb6bf197f189 /boot           ext4    defaults        0       2
# /boot/efi was on /dev/sda1 during installation
UUID=D45C-6FEC  /boot/efi       vfat    umask=0077      0       1
```

## LVM on LUKS (not recommended)
```
root@pve:~# lsblk
NAME              MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda                 8:0    0   3.6T  0 disk  
├─sda1              8:1    0   512M  0 part  /boot/efi
├─sda2              8:2    0   954M  0 part  /boot
└─sda3              8:3    0   3.6T  0 part  
  └─sda3_crypt    253:0    0   3.6T  0 crypt 
    └─vg0-lv_root 253:2    0   5.5T  0 lvm   /
sdb                 8:16   0  16.4T  0 disk  
└─sdb1              8:17   0  16.4T  0 part  
  └─hdd-1         253:3    0  16.4T  0 crypt /mnt/hdd-1
sdc                 8:32   1 114.6G  0 disk  
├─sdc1              8:33   1 114.5G  0 part  
└─sdc2              8:34   1    32M  0 part  
sdd                 8:48   0   1.8T  0 disk  
└─sdd1              8:49   0   1.8T  0 part  
  └─sdc1_crypt    253:1    0   1.8T  0 crypt 
    └─vg0-lv_root 253:2    0   5.5T  0 lvm   /

root@pve:~# cat /etc/fstab
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/vg0-lv_root /               ext4    errors=remount-ro 0       1
# /boot was on /dev/sda2 during installation
UUID=b10fd413-2428-403b-a753-70097220c4ee /boot           ext4    defaults        0       2
# /boot/efi was on /dev/sda1 during installation
UUID=8F0E-24E8  /boot/efi       vfat    umask=0077      0       1
```

see also:
Pictures in `/static/proxmox-installation-archive`

# /etc/hosts - RELEVANT for installation
(https://forum.proxmox.com/threads/problem-with-installing-proxmox-ve-on-debian.44316/)
```
root@pve:~# cat /etc/hosts
127.0.0.1       localhost
#dont uncommet - 127.0.1.1      pve.fritz.box   pve
10.0.1.10       pve
```

# Email Setup
https://forum.proxmox.com/threads/mail-versand-einrichten.69501/
- https://www.cyberciti.biz/faq/how-to-configure-postfix-relayhost-smarthost-to-send-email-using-an-external-smptd/
- https://www.thomas-krenn.com/de/wiki/TKmon_Relayhost_mit_SMTP_AUTH
https://forum.proxmox.com/threads/get-postfix-to-send-notifications-email-externally.59940/

# Unlock PVE remotely
```
ssh root@10.0.1.10 -i .ssh/owlnest-env_rsa
# cryptroot-unlock
```

# NFS Share
create NFS share on server:
```
sudo nano /etc/export
```
```
/nfs/publicfun 10.0.1.0/24(rw,sync,no_subtree_check)
```
mount shared directory on client:
```
sudo mount -t nfs 10.0.1.12:/nfs/publicfun /mnt/test
```
setup nfs:
https://www.howtoforge.com/tutorial/how-to-configure-a-nfs-server-and-mount-nfs-shares-on-ubuntu-18.04/
https://www.thegeekdiary.com/basic-nfs-security-nfs-no_root_squash-and-suid/

# freeIPA
### Howto secure NFS
> look at: "Secure NFS" uses DES encryption 

https://www.linuxjournal.com/content/encrypting-nfsv4-stunnel-tls
https://www.golinuxcloud.com/nfs-exports-options-examples/
https://www.ibm.com/docs/en/aix/7.1?topic=management-nfs-v4-host-authentication

Kerberos:
https://help.ubuntu.com/community/NFSv4Howto
https://www.tecmint.com/setting-up-nfs-server-with-kerberos-based-authentication/
https://wiki.ubuntuusers.de/Kerberos/NFS_mit_Kerberos_sichern/
https://www.golinuxcloud.com/configure-secure-kerberized-nfs-share-linux/
