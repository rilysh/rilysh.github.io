---
layout: post
title: My Void Linux setup
subtitle: Setting up Void Linux
tags: [guide]
---

## Setup Void Linux
Void is another general-purpose GNU/Linux distro that is built from the ground up. It's not a derivative or fork of any existing GNU/Linux distro.

## Index
1. [Get Void ISO](#download-iso)
2. [Insert to a USB](#insert-to-usb)
3. [Installation](#installation)
4. [Post Installation](#post-installation)
5. [Install X11 and Xfce](#install-x11-and-xfce)
6. [Install and configure login manager](#install-and-configure-login-manager)
7. [Configure The System](#configure-the-system)
8. [Install other software](#install-other-software)

### Download ISO
Here I used [base live](https://voidlinux.org/download/) ISO image (glibc version). Users are recommended to use glibc, and only use musl unless you know what you're doing.

Don't use Xfce live ISO image, as it misses several Xfce components that you probably need in the future. Also, default X11 fonts are stripped down from Xfce ISO.

### Insert to USB
Grab a USB pen drive or SD card attached card reader and plug it into your PC. Make sure there aren't any important files on that USB as we need to format the drive. After getting this, grab one of the software to make the USB flashable.

i. [balenaEtcher](https://www.balena.io/etcher/) (Linux, Windows, macOS)\
ii. [Ventoy](https://github.com/ventoy/Ventoy/releases) (Linux, Windows)

Both are recommended software, however, both work with a completely different approach, in short, balenaEtcher will flash one ISO per USB pen drive whereas, with Ventoy, you can flash multiple ISO with it. (See their websites and GitHub README for more information).

### Installation
Installation of Void is pretty straightforward. After booting Void live base image on your PC, you should see a tty. Now login as `root` user with password `voidlinux` and run `void-installer`.\
For this installation part, see [this](https://docs.voidlinux.org/installation/live-images/guide.html).

### Post Installation
When the installation finishes, reboot the PC (and remove the drive that was used to install Void). Now login to that tty with your username and password.\
We need to check does our network is working fine or not. If you're on an ethernet connection, it's likely will be connected automatically without any hassle.\
Test network connection by pinging, `ping duckduckgo.com` (use CTRL+C to exit from it).

If you're on a wireless connection, you've to configure the connection.\
```sh
# This make sure that the connection interface isn't blocked
rfkill unblock wlan

# Check if the wireless interface is connected properly and listed
ip link set [interface] up

# Configure the wireless network
wpa_passphrase [SSID] [passphrase] >>/etc/wpa_supplicant/wpa_supplicant.conf
wpa_supplicant -B -i [interface] -c /etc/wpa_supplicant/wpa_supplicant.conf

# Disable dhcpcd's IPv4 addressing
echo "noipv4ll" >>/etc/dhcpcd.conf

# Rebind the interface
dhcpcd -n
```
P.S. If necessary you may need to reboot the PC after configuring the wireless connection.

### Install X11 and Xfce
To get a proper desktop environment, we need to install X11 (display server) and Xfce (desktop environment).\
It's also a pretty much straightforward job!\
```sh
# Install Xorg and Xfce4
sudo xbps-install -Sy xorg xfce4
```
Installing full Xorg pulls all X11 packages, including several X11 bitmap fonts, which are necessary for most users.

### Install and configure the login manager
For login manager, I personally like lightdm, as it's minimal and have a pretty GUI style.
```sh
# Invoke these instructions to get it
sudo xbps-install -Sy lightdm lightdm-gtk3-greeter

# Install Vim (we need it)
sudo xbps-install -Sy vim

# If you prefer something much easier then
sudo xbps-install -Sy nano
```
Lightdm is installed but we need to configure it. First open `/etc/lightdm/lightdm.conf` file with vim (or nano) (root priviledges required).
```sh
sudo vim /etc/lightdm/lightdm.conf
```
Go to the "Seat" section and uncomment `greeter-session` and set the value something like this `greeter-session=lightdm-gtk-greeter`\
Save and exit from Vim and create a symbolic link of lightdm service in `/var/service`
```sh
# root privileges are required
sudo ln -s /etc/sv/lightdm /var/service
```
If you follow as I wrote here, you now will see a login screen asking for your username and password.

### Configure The System
##### .xinitrc
Create a `.xinitrc` file in your home directory, which executes `startxfce4`.
```sh
echo "exec startxfce4" >~/.xinitrc
```

##### dbus
Without DBUS service, many Xfce plugin will just crash, since they can't communicate with each other without it.
```sh
# Install dbus (if not exist, or necessary)
sudo xbps-install -Sy dbus

# Create a symbolic link of dbus in /var/service
sudo ln -s /etc/sv/dbus /var/service
```

##### Network Manager
By default network manager will not be configured, so we have to.
```sh
# Install network manager applet
sudo xbps-install -Sy network-manager-applet

# Create a symbolic link of network manager in /var/service
sudo ln -s /etc/sv/NetworkManager /var/service
```

##### Disable bitmap fonts Firefox
Without disabling bitmap fonts, Firefox fonts will appear like quite old style rough fonts (like in Windows XP).
```sh
sudo ln -s /usr/share/fontconfig/conf.avail/70-no-bitmaps.conf /etc/fonts/conf.d
```

##### Fix misleading time
Sometimes default timezone may not show your current time, even if you link your timezone in `/etc/localtime`. To get time accuracy, we'll use an NTP daemon.
```sh
# I prefer OpenNTPD, however, you can choose whatever you like from https://docs.voidlinux.org/config/date-time.html
sudo xbps-install -S openntpd
sudo ln -s /etc/sv/openntpd /var/service
```
Click on the time applet again and see if the time it's updated now. To verify, go to `https://time.is` website.

Note: If you found the time that OpenNTPD is showing is wrong, please change the server according to your location. To get a server list see [here](https://www.pool.ntp.org/zone/@).\
Open `/etc/ntpd.conf` with vim and change this portion.
```conf
servers pool.ntp.org
server <your_country_code.pool.ntp.org> # Example: jp.pool.ntp.org
sensor *
```
Issue: If by doing anything the time didn't fix, try to reset BIOS time. Void reads the hardware clock first, so if OS time isn't exactly synchronized with hardware time, it will conflict there. For some reason, if you didn't find the time resetting feature in the BIOS, the best way to reset is to remove the CMOS battery and insert it back after ten minutes. For more info, see below.
[link 1](https://www.ibm.com/docs/en/flashsystem-7x00/8.3.x?topic=parts-removing-replacing-cmos-battery)
[link 2](https://www.hp.com/us-en/shop/tech-takes/what-is-cmos-battery-how-to-remove-and-replace)
[link 3](https://www.repairwin.com/fix-pc-loses-time-settings-4-ways-to-resolve-the-problem/)

You don't need to replace the CMOS battery, but just pull it from there and hold on for ten minutes and insert it back again!\
**Warning**: By removing the CMOS battery it will also reset your BIOS settings, so you may have to set up your BIOS again accordingly!

##### Show other drives or partitions
By default Void won't mount your external drives or your HDD partitions, so we'll make them mount automatically whenever we boot into our system.
```sh
# Create a or more directories with a unique name
# For me, I've one HDD and in that HDD I've 3 partitions
sudo mkdir /media/alpha
sudo mkdir /media/charlie
sudo mkdir /media/delta
```
Now mount them to these directories
```sh
# Mount /dev/sdX (In my case, I've 3 partitions)
sudo mount /dev/sdX /media/alpha
sudo mount /dev/sdY /media/charlie
sudo mount /dev/sdZ /media/delta
```
Open thunar, and if you can see partitions or drives in the "Devices" section.\
We have to make them permanent, so open `/etc/fstab` and address devices accordingly.
```sh
# Run blkid to get UUID of these devices
blkid

# Copy the UUID and paste them in fstab with the following format
UUID=<COPIED_UUID> <MOUNTED_PATH> <FS_TYPE> defaults <BCK_OP FS_ORDER>

# COPIED_UUID - The UUID you get from blkid (make sure to copy for those secondary drives only).
# MOUNTED_PATH - The path where we mounted these devices first.
# FS_TYPE - What filesystem those devices have? For example, ext4, btrfs, etc.
# BCK_OP - Enable backup or not (1 = Enabled, 0 = Disabled)
# FS_ORDER - Should fsck check those devices (partitions or drives)? (1 = Enabled, 0 = Disabled)

# Sample fstab
UUID=75e7d680-514e-4f5e-b880-458542b5013f /media/charlie ext4 defaults 0 0
```

GVFS provides a layer to interact with the filesystem. Thunar needs it to extend several functionalities. Use the following instruction to install it.
```sh
# gvfs for computer drives, partitions
sudo xbps-install -S gvfs

# gvfs-mtp for mobile device support
sudo xbps-install -S gvfs-mtp
```

##### Support webp thumbnail
Xfce and Thunar can't recognize webp format images.
```sh
# Install webp devel (included webp format and development libs)
sudo xbps-install -S libwebp-devel

# Install webp-pixel-buffer loader
sudo xbps-install -S webp-pixbuf-loader
```

##### Disable BTRFS and LVM (advanced)
If you didn't set up BTRFS or LVM on installation or don't want to use BTRFS or LVM anywhere, you can remove both of them to reduce boot time.
```sh
# Ignore these packages
echo "ignorepkg=btrfs-progs" | sudo tee /etc/xbps.d/10-ignores.conf

# Now remove btrfs and lvm
sudo xbps-remove -Ry btrfs-progs lvm2
```

##### Disable watchdog (advanced)
Watchdog is used to check if the system is frozen so the OS will reboot eventually. However, this feature is unnecessary for a desktop or laptop user, as we'll likely reboot if something happens.

Open `/etc/default/grub` with Vim (or nano) and append the following parameter
```sh
# Using vim
sudo vim /etc/default/grub

# Disable watchdog in grub
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=0 nowatchdog" # Remember to append it after first parameter and separate them with a space
# Save and exit
```
Now update the grub config with
```sh
sudo grub-mkconfig
```
### Install other software
We got DE now we need several other software.\
Note: You can skip any of them if you want.

i. If you want multilib support (32-bit support), please install the following package
```sh
sudo xbps-install -Sy void-repo-multilib
# Update the system and the XBPS database
sudo xbps-install -Suy
```
ii. If you want multilib non-free (proprietary) 32-bit support, please install the following package
```sh
sudo xbps-install -Sy void-repo-multilib-nonfree
# Update the system and the XBPS database
sudo xbps-install -Suy
```
iii. If you want some non-free (proprietary) apps, please install the following package
```sh
sudo xbps-install -Sy void-repo-nonfree
# Update the system and the XBPS database
sudo xbps-install -Suy
```

```sh
# You can omit any one, two, or whole
sudo xbps-install -S 7zip element-desktop firefox fish-shell flameshot git github-cli gparted htop krita lightdm-gtk-greeter-settings man-pages-devel mpv pass qView sakura vscode unzip xtools
```