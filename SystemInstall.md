# System Install

- [Loadkeys](#loadkeys)
- [Partition the Hard Drive](#partition-the-hard-drive)
  - [Create Filesystem](#create-filesystem)
- [Mount the Filesystem](#mount-the-filesystem)
- [Mount the boot](#mount-the-boot)
  - [Create Swap Space](#create-swap-space)
- [Mount Swap Space](#mount-swap-space)
- [Install the Base System](#install-the-base-system)
- [Generate the fstab File](#generate-the-fstab-file)
- [Chroot into Arch Linux](#chroot-into-arch-linux)
- [Set the Time Zone](#set-the-time-zone)
- [Generate Locale File](#generate-locale-file)
- [Install a Boot Loader](#install-a-boot-loader)
- [Reboot](#reboot)

## Loadkeys

    loadkeys fr


## Partition the Hard Drive

    fdisk -l
    cfdisk /dev/sda
    -> dos
    -> sda1 boot 512M
    -> sda2 swap 2G
    -> sda3
    fdisk -l

### Create Filesystem
    mkfs.ext4 /dev/sda3
    mkfs.ext4 /dev/sda1

## Mount the Filesystem
    mount /dev/sda3 /mnt
## Mount the boot
    mkdir /mnt/boot
    mount /dev/sda1 /mnt/boot

### Create Swap Space
    mkswap /dev/sda2
## Mount Swap Space
    swapon /dev/sda2



## Install the Base System
    pacstrap /mnt base base-devel


## Generate the fstab File
    genfstab -U /mnt >> /mnt/etc/fstab
    cat /mnt/etc/fstab

## Chroot into Arch Linux
    arch-chroot /mnt


## Set the Time Zone
    ln -sf /usr/share/zoneinfo/Europe/Brussels /etc/localtime
    hwclock --systohc


## Generate Locale File
Uncomment  in => /etc/locale.gen

    locale-gen
    echo "LANG=en_GB.UTF-8" > /etc/locale.conf
    echo "LANG=en_GB.utf8" > /etc/environment
    cat /etc/vconsole.conf
    KEYMAP=be-latin1
    echo "HOSTNAME" > /etc/hostname


    systemctl enable dhcpcd


## Install a Boot Loader
    pacman -S grub os-prober
    grub-install /dev/sda
    grub-mkconfig -o /boot/grub/grub.cfg

    passwd

## Reboot
