# System Install

- [System Install](#system-install)
  - [System Checks](#system-checks)
    - [Check internet connection](#check-internet-connection)
    - [Verify the boot mode](#verify-the-boot-mode)
  - [Loadkeys](#loadkeys)
  - [Partition the Hard Drive](#partition-the-hard-drive)
    - [Structure](#structure)
    - [Command](#command)
    - [Create Filesystem](#create-filesystem)
  - [Mount the Filesystem](#mount-the-filesystem)
    - [Create Swap Space](#create-swap-space)
  - [Mount Swap Space](#mount-swap-space)
  - [Install the Base System](#install-the-base-system)
    - [enable Arch Multilib](#enable-arch-multilib)
    - [Install](#install)
  - [Generate the fstab File](#generate-the-fstab-file)
  - [Chroot into Arch Linux](#chroot-into-arch-linux)
  - [Set the Time Zone](#set-the-time-zone)
  - [Generate Locale File](#generate-locale-file)
  - [Set HOSTNAME](#set-hostname)
  - [Enable DHCP](#enable-dhcp)
  - [Mount the boot](#mount-the-boot)
  - [Install a Boot Loader](#install-a-boot-loader)
  - [Set root password](#set-root-password)
  - [Reboot](#reboot)


## System Checks 

### Check internet connection 

    ifconfig
    ping -c2 google.com

### Verify the boot mode

    ls /sys/firmware/efi/efivars

## Loadkeys

    loadkeys fr


## Partition the Hard Drive

### Structure 
|Mount point|Partition|Partition type|Suggested size|
|-|-|-|-|
|/mnt/boot or /mnt/efi|/dev/sdX1|EFI system partition|260–512 MiB|
|[SWAP]|/dev/sdX2|Linux swap|2GB|
|/mnt|/dev/sdX3|Linux x86-64 root (/)|Remainder of the device|


### Command 

> List disk 
> 
    lsblk

    fdisk -l

> Edit partition 
> 
    cfdisk /dev/sda
    -> gpt
    -> sda1 EFI System  512M
    -> sda2 swap        2G
    -> sda3 Linux FS
    fdisk -l

### Create Filesystem
    mkfs.fat -F32 /dev/sda1
    mkfs.ext4 /dev/sda3
    

## Mount the Filesystem
    mount /dev/sda3 /mnt

### Create Swap Space
    mkswap /dev/sda2
## Mount Swap Space
    swapon /dev/sda2



## Install the Base System

### enable Arch Multilib

    [multilib]
    Include = /etc/pacman.d/mirrorlist

### Install

    pacstrap /mnt base base-devel linux linux-firmware nano dhcpcd


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
 ## Set HOSTNAME
    echo "HOSTNAME" > /etc/hostname

## Enable DHCP
    systemctl enable dhcpcd

## Mount the boot
    mkdir /boot/efi
    mount /dev/sda1 /boot/efi

## Install a Boot Loader

    pacman -S grub efibootmgr dosfstools os-prober mtools
    grub-install --target=x86_64-efi  --bootloader-id=grub_uefi --efi-directory=/boot/efi  --recheck
    grub-mkconfig -o /boot/grub/grub.cfg

## Set root password 

    passwd

## Reboot

    exit 
    umount -a
    poweroff