# Server Basics - System Installation and First Steps #

## Basic Info ##

**Description:** Installation and first actions on a fresh system  
**System OS:** Ubuntu Server 16.04 LTS (x86_64)  
**Date:** 2017.11.09

## Choose hostname ##

In non-enterprise setups we tend to neglect giving meaningful names in our systems.

Decide on a domain name under which the hosts are assigned. That doesn't need to be a registered one if you do not intend to serve the Internet.

Then choose a nomenclature such as colors (i.e. blue, red, green) or Heroes (i.e. Odysseus, Theseus, Hercules) to name your systems.

**Example**  
_Domain:_ mylocaldomain.com  
_Hostname:_ theseus.mylocaldomain.com

## Software Selection ##

- OpenSSH server is absolutely required for managing the new system

---

## Update OS base ##

```sh
sudo apt-get update
sudo apt-get upgrade
```

## Set correct time zone ##

```sh
sudo dpkg-reconfigure tzdata
```

## Set correct locales ##

```sh
sudo locale-gen en_US en_US.UTF-8 el_GR.UTF-8
sudo dpkg-reconfigure locales
```

## Check hostname ##

Display system's host name

```sh
hostname
```

Display the FQDN (Fully Qualified Domain Name). A FQDN consists of a short host name and the DNS domain name.

```sh
hostname -f
```

## Set hostname ##

```sh
sudo hostnamectl set-hostname <hostname>
```

## Install some utilities ##

```sh
sudo apt-get install ntp
sudo apt-get install nano htop iftop
sudo apt-get install curl zip
```

## Add new user ##

```sh
adduser <username>
usermod -aG sudo <username>
```

## Install some libraries ##

```sh
sudo apt-get install python-software-properties
sudo apt-get install build-essential
```

## Tips ##

-
-
-
