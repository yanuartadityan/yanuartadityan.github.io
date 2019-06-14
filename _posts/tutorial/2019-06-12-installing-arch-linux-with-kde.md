---
title: 'Arch Linux Installation w/ KDE Plasma DE'
excerpt: "Quick step by step on installing Arch Linux"
category: tutorial
layout: post
comments: true
tags: [tutorial, linux]
---

In short, here is the complete list to get the minimalistic Arch Linux with latest KDE Plasma minimum desktop environment. **Note** This is living post, therefore updates will be added in the future, mostly adding explanation with illustration/screenshots on each command.

Make sure to check if there is internet connection to the Internet . Do `ping 8.8.8.8` to see. 

Install our most favorite text editor of course `pacman -S vim`.

Check your disk structure `fdisk -l` and create partition by `cfdisk /dev/sda`. Check DOS instead of GPT if you don't have existing Bootloader. If you have existing drive/partition which host Windows with UEFI, usually the bootloader is listed also with *cfdisk*.

Add propietary format to your existing partitions `mkfs.ext4 /dev/sda*`.

Enable memory swap if your system has less RAM (mine is 16GB and I do not have swap) `mkswap /dev/sda*swap` then `swapon /dev/sda*swap`.

Mount the root to the first partition `mount /dev/sda1 /mnt`.

Create the ~/ or home folder inside the root `mkdir /mnt/home` then mount the second partition to the home folder `mount /dev/sda2/ /mnt/home`.

Install the base and base-devel Arch Linux package to the system `pacstrap /mnt base base-devel`.

Generate genfstab file `genfstab /mnt >> /mnt/etc/fstab`.

Create root access on using bash shell `arch-chroot /mnt /bin/bash`.

Create locale.ge file `vim /etc/locale.gen` then enable it `locale-gen`.
 
Configure the language the system will use `vim /etc/locale.conf` and for me, I use generic English `LANG=en_US.UTF-8`.

Check all available zone info`ls /usr/share/zoneinfo` and enable on your closest area `ln -s /usr/share/zoneinfo/Europe/Stockholm`.

Then sync the system clock to the area you've chosen `hwclock --systoch --utc`.

Setup password for the root `passwd`.

Add your computer name as the default hostname `vim /etc/hostname` and add `[computername]`.

Make sure that DHCP is enabled when you opt so `systemctl enable dhcpcd`.

Before we boot, we must install the bootloader. For this we use GRUB `pacman -S grub os-prober`.

Tell GRUB what drive it should seek for the bootloader `grub-install /dev/sda` and create the config file `grub-mkconfig -o /boot/grub/grub.cf`.

Reboot the system `sudo reboot`.

Now, we gotta add new user (non root) and add it as sudoers. Open `visudo`, then uncomment `# %wheel ALL=(ALL)  ALL`. It means, all members of wheel group will be able to be invoke bash with sudo access.

Add your new user `useradd -m [username]`, and set its password `passwd [username]`.

Add the user to the wheel group `usermod -aG wheel [username]`. Then reboot.

For me, I like KDE Plasma due to balance between simplicity, package repository, stability and also maturity. Some people might like Cinnamon for GUI, XFCE for light and robustness and many other. For me, I took `sudo pacman -S plasma plasma-meta` which will install the most minimum setup of KDE Plasma 5.x.x. Then install the desktop `sudo pacman -S plasma-desktop`.

Now, install necessary applications for KDE, e.g., browser, file explorer and simple text editor `sudo pacman -S konsole dolphin firefox kate`. For complete KDE applications, you can install all package by `sudo pacman -S kde-applications` but it will take around 2.5GB to install.

Install desktop manager for KDE, `sudo pacman -S sddm` and enable it `sudo systemctl enable sddm`.

Init `sudo pacman -S xorg xorg-init` before the last reboot, then restart `sudo reboot` and the you should be prompted with the first GUI KDE Plasma on Arch Linux.

Cheers.

