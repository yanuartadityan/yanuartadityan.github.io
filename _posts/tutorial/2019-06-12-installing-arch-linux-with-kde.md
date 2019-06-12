---
title: 'Arch Linux Installation w/ KDE Plasma DE'
excerpt: "Quick step by step on installing Arch Linux"
category: tutorial
layout: post
comments: true
tags: [tutorial, linux]
---

`ping 8.8.8.8`

`pacman -S vim`

`fdisk -l`

`cfdisk /dev/sda`

`mkfs.ext4 /dev/sda*`

`mkswap /dev/sda*swap`

`swapon /dev/sda*swap`

`mount /dev/sda1 /mnt`

`mkdir /mnt/home`

`mount /dev/sda2/ /mnt/home`

`pacstrap /mnt base base-devel`

`genfstab /mnt >> /mnt/etc/fstab`

`arch-chroot /mnt /bin/bash`

`vim /etc/locale.gen`

`locale-gen`
 
`vim /etc/locale.conf` and add `LANG=en_US.UTF-8`

`ls /usr/share/zoneinfo`

`ln -s /usr/share/zoneinfo/Europe/Stockholm`

`hwclock --systoch --utc`

`passwd`

`vim /etc/hostname` and add `[computername]`

`systemctl enable dhcpcd`

`pacman -S grub os-prober`

`grub-install /dev/sda`

`grub-mkconfig -o /boot/grub/grub.cf`

`sudo reboot`

`visudo`

`useradd -m [username]`

`passwd [username]`

`usermod -aG wheel [username]`

`sudo pacman -S plasma`

`sudo pacman -S plasma-meta`

`sudo pacman -S konsole dolphin firefox kate`

`sudo pacman -S xorg xorg-init`

`sudo pacman -S plasma-desktop`

