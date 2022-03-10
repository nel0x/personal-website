+++
title = "Setup file sharing with SMB"
date = "2022-03-10T16:00:00+01:00"
tags  = ['network', 'smb', 'file-sharing', 'tech']
+++

Having hundreds of gigabytes and sometimes even multiple terabytes can get fastly very challanging for regular transfers and backups. Therefore we can make management very easy when instead using network storage, which only needs to be mounted on the corresponding machines. The following should serve as a cheatsheet for quickly setting up samba shares on the home network.

# Install and setup samba
```
$ sudo apt update && sudo apt install samba
```

For security disable Samba NetBIOS service, as we won't use it.
```
$ sudo systemctl disable --now nmbd.service
```

To avoid security issues that can arise from running an unconfigured, network-enabled service, let’s stop the Samba server until configuration details are in place:
```
$ sudo systemctl stop smbd.service
```

# Configure Samba global
```
$ sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak && sudo nano /etc/samba/smb.conf
```
```
[global]
        server string = samba_server
        server role = standalone server
        bind interfaces only = yes
        disable netbios = yes
        smb ports = 445
        log file = /var/log/samba/smb.log
        max log size = 10000
```
after editing `smb.conf` always run `testparm` to check that there are no syntax errors.
If testparm reports *Loaded services file OK.*, then there are no syntax errors that would stop the Samba server from starting.

# Setup Samba user & share
### Create Samba root directory
```
$ sudo mkdir /samba/
$ sudo chown :sambashare /samba/
```
### Create Samba user
```
$ sudo mkdir /samba/joe
$ sudo adduser --home /samba/joe --no-create-home --shell /usr/sbin/nologin --ingroup sambashare joe

$ sudo chown joe:sambashare /samba/joe/
$ sudo chmod 2770 /samba/joe/

$ sudo smbpasswd -a joe
$ sudo smbpasswd -e joe
```
### Setup share
```
[joe]
        path = /samba/joe
        browseable = no
        read only = no
        force create mode = 0660
        force directory mode = 2770
        valid users = joe
```
```
$ testparm && sudo systemctl start smbd
```

# Configure Samba Clients
```
$ sudo apt update && sudo apt install smbclient cifs-utils
```
Accessing Samba share temporarily:
```
$ smbclient //10.0.1.18/joe -U joe
```
You can mount a samba share to a directory in your local Linux system using the mount and cifs type option.
```
$ mkdir /mnt/smb
$ sudo mount -t cifs -o username=joe //10.0.1.18/joe /mnt/smb
$ df -h
```

### Mount Samba share using fstab.

You can use fstab file to persist Samba shares mounting through system reboots. In my example, I have the following line added to the end of `/ect/fstab` file.
```
//10.0.1.18/joe /mnt/smb cifs credentials=/.smbcreds 0 0
```
```
$ cat << EOF > /.smbcreds
username=joe
password=supersecretpassword
domain=WORKGROUP
EOF
```

To test:
```
$ sudo mkdir -p /mnt/smb
$ sudo mount -a
$ df -hT | grep cifs
```

### Repos:
- https://www.digitalocean.com/community/tutorials/how-to-set-up-a-samba-share-for-a-small-organization-on-ubuntu-16-04
- https://computingforgeeks.com/install-and-configure-samba-server-share-on-ubuntu/
