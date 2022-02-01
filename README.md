# Macbook Air Linux (Debian)

Installing debian on Macbook Air 2013


## Install Debian 

Download Debian 10 ISO file from [official Debian website](https://www.debian.org/releases/buster/debian-installer/)

1. Download the Debian 10 ISO file
2. Create a bootable USB
3. Boot the system from USB
4. Select "Graphical Debian Installer" from Main Menu
5. Install

_Most probably wi-fi is not working, so you will need to use external wi-fi usb or your phone to access the internet for now._


## Install Desktop Environment

### Pantheon Desktop
I choose Elementary OS's Pantheon Desktop: [Pantheon Desktop on Debian](https://gandalfn.ovh/) 

#### Install Pantheon Desktop
1. Install required dependencies if needed
    `apt-get install apt-transport-https software-properties-common wget`
2. Download package file
    `wget https://gandalfn.ovh/debian/pool/main/p/pantheon-debian-repos/pantheon-debian-repos_5.0-0+pantheon+buster+juno1_all.deb`
3. Install package
    `dpkg -i pantheon-debian-repos_5.0-0+pantheon+buster+juno1_all.deb`
4. Update your repositories
    `sudo apt update`
5. Install Pantheon
    `sudo apt install pantheon`

#### Launch Pantheon Desktop

To launch pantheon desktop select pantheon when launching session on greeter.


## Possible Missing Firmware

Fixing missing firmware

```sh
# sudo apt update
# sudo apt install apt-file
# sudo apt-file update
# sudo apt-file search bxt_dmc
```

The package `firmware-misc-nonfree` provides the missing firmware.

Installing the `firmware-linux` package solve the problem because it depends on `firmware-misc-nonfree`.

To install it add `non-free` to your `/etc/apt/source.list`, append to end of existing lines.

```
deb http://deb.debian.org/debian buster main contrib non-free
deb http://deb.debian.org/debian-security/ buster/updates main contrib non-free
deb http://deb.debian.org/debian buster-updates main contrib non-free
```

Now install `firmware-linux`
```sh
# sudo apt-get install firmware-linux
```

## Fix Wi-Fi

First of all, confirm that you have a Broadcom adapter:
```sh
# lspci -vvnn | grep -A 9 Network
```

To enable wi-fi, you need to do the following:

```sh
# sudo apt-get update
# sudo apt-get --reinstall install bcmwl-kernel-source
# sudo modprobe -r b43 ssb wl brcmfmac brcmsmac bcma
# sudo modprobe wl
```

Test wi-fi, remove external wi-fi dongle or your phone. Connect to network.

## Fix Camera

The MacBook camera is always Facetime HD. To activate it, run these commands one by one in terminal:

```sh
# sudo apt-get install git
# sudo apt-get install curl cpio
# git clone https://github.com/patjak/facetimehd-firmware.git
# cd facetimehd-firmware
# make
# sudo make install
# cd ..
# sudo apt-get install kmod libssl-dev checkinstall
# git clone https://github.com/patjak/bcwc_pcie.git
# cd bcwc_pcie
# make
# sudo make install
# sudo depmod
# sudo modprobe -r bdc_pci
# sudo modprobe facetimehd
# sudo nano /etc/modules
```

Add `facetimehd` to module list `/etc/modules`

```sh
# sudo echo facetimehd >> /etc/modules
```

Test camera

```sh
# mplayer tv://
```

