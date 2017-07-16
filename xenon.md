# Xenon

Xenon is a HP EliteBook 840 with an i7-5500U (2.4Ghz), 8GB RAM, 240GB SSD

## The naming
*Xenon* is a chemical element with symbol Xe and atomic number 54. It's a noble gas and is generally unreactive.

## Rough setup and configuration

Instructions for the base install of Arch Linux

## Create base installer

- Download latest iso from https://www.archlinux.org/download/

## Install Base System
- Install base system with instructions from Erik Dubois (http://erikdubois.be/how-to-install-arch-linux/)

Change keyboard layout if necessary
```
loadkeys be-latin1
```
Partition hard disk
```
cfdisk /dev/sda
```
delete all existing partitions
sda1 / allocate all space (exept swap), make it primary __and__ bootable

sda2 /swap (your mileage may vary but I take the same amount as RAM), primary, type 82 for swap

Write partition table to disk

Make filesystems and mount partitions
```
mkfs.ext4 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda
mount /dev/sda1 /mnt
```
Start installing archlinux
```
pacstrap -i /mnt base base-devel
```

Start configuration
```
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt /bin/bash
nano /etc/locale.gen
```
uncomment these lines

be_BY.UTF8

en_GB.UTF8

en_US.UTF8

CTRL + X , yes, enter

```
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8
nano /etc/vconsole.conf
```

KEYMAP=be-latin1
 FONT=lat9w-16

CTRL + X , yes, enter

```
rm /etc/localtime
ln -s /usr/share/zoneinfo/Europe/Brussels /etc/localtime
hwclock --systohc --utc
echo *hostname* > /etc/hostname
nano /etc/hosts
```

Append chosen hostname at the end of lines

```
pacman -S networkmanager
systemctl enable NetworkManager
mkinitcpio -p linux
passwd
pacman -S grub
grub-install --target=i386-pc --recheck /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
umount /dev/sda1
exit
reboot
```

## Changelog
- nothing yet