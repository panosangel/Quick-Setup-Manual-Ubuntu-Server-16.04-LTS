# File Server - SAMBA #

## Basic Info ##

**Description:** Install SAMBA  
**System OS:** Ubuntu Server 16.04 LTS (x86_64)  
**SAMBA version:** 4.3.11  
**Date:** 2017.06.09

**Read more:** [Ubuntu Official Documentation - File Server](https://help.ubuntu.com/lts/serverguide/samba-fileserver.html)  
**Source:** [Samba Server installation on Ubuntu 16.04 LTS](https://www.howtoforge.com/tutorial/samba-server-ubuntu-16-04/)  
**Source:** [How to Install and Configure Samba Server on Ubuntu 16.04 for File Sharing](https://www.linuxbabe.com/ubuntu/install-samba-server-ubuntu-16-04)  
**Source:** [YouTube - Traversy Media - Ubuntu Server 14.04 Setup Part 4 - Samba File Server](https://www.youtube.com/watch?v=4Ml29lCVj48)  
**Read more:** [Install Samba Server Ubuntu 16.04 (Xenial Xerus) server](http://www.ubuntugeek.com/install-samba-server-ubuntu-16-04-xenial-xerus-server.html)  
**Read more:** [How to Create a Network Share Via Samba Via CLI (Command-line interface/Linux Terminal) - Uncomplicated, Simple and Brief Way!](https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated%2C%20Simple%20and%20Brief%20Way%21)

## Install SAMBA on host ##

Install required packages:
```sh
sudo apt-get install samba
```

**Optional:** Install graphical (GUI) interface to control SAMBA:

```sh
sudo apt-get install system-config-samba
```

At this point it is suggested to backup the configuration file.

```sh
sudo cp -pf /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

## Actions on Windows machine (Guest) ##

1) Check Window's Guest Group.

```
net config workstation
```

2) Set SAMBAS's server host entry.

Change entry to fit your preference.

```
notepad C:\\Windows\System32\drivers\etc\hosts
```

```
[...]
192.168.X.X    samba.linuxhost.local    samba
```

## Create SAMBA's Anonymous Folder ##

```sh
sudo mkdir -p /home/samba/anonymous
sudo chmod -R 0777 /home/samba/anonymous
sudo chown -R nobody:nogroup /home/samba/anonymous
```

## Configure SAMBA - Global & Anonymous ##

```sh
sudo nano /etc/samba/smb.conf
```

```
[global]
workgroup = WORKGROUP
server string = Samba Server %v
netbios name = mint
security = user
map to guest = bad user
dns proxy = no

#============================ Share Definitions ==============================

[Anonymous]
comment = Anonymous Public Folder
path = /home/samba/anonymous
browsable =yes
writable = yes
guest ok = yes
read only = no
force user = nobody
create mask = 0777
```

Restart service to apply changes

```sh
sudo service smbd restart
```

## Create SAMBA's Secured Folder ##

Add user (smbusr) and group (smbgrp) into the system.

```sh
sudo addgroup smbgrp
sudo useradd smbusr -G smbgrp
sudo smbpasswd -a smbusr
```

Create the following folder structure.

```sh
sudo mkdir -p /home/samba/secured
cd /home/samba
sudo chmod -R 0770 secured
sudo chown root:smbgrp secured
```

## Configure SAMBA - Secured ##

```sh
sudo nano /etc/samba/smb.conf
```

```
[...]
[Secured]
comment = Secured Public Folder
path = /home/samba/secured
valid users = @smbgrp
browsable = yes
writable = yes
guest ok = no
create mask = 0770
```

## Restart services ##

```sh
sudo service smbd restart
sudo service nmbd restart
```

## Test configuration ##

```sh
testparm
```

## Windows Client ##

At the Windows machine, open the "\\samba" network device again, it will request a username and password now. Enter the user details that you created above. In this case, the values were user=smbusr and password=123456.
