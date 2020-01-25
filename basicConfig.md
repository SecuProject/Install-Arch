<h1> Basic Config </h1>

- [Install vmware tools](#install-vmware-tools)
  - [Service responsible for the Virtual Machine status report](#service-responsible-for-the-virtual-machine-status-report)
  - [Filesystem utility. Enables drag & drop functionality between host and guest through FUSE](#filesystem-utility-enables-drag--drop-functionality-between-host-and-guest-through-fuse)
- [For xorg](#for-xorg)
- [SSH](#ssh)
- [Add user to sudo](#add-user-to-sudo)
- [Tools](#tools)
- [Update your system](#update-your-system)
  - [Update System](#update-system)
- [Iptables](#iptables)
  - [Set policy to DROP](#set-policy-to-drop)
  - [Set DNS server](#set-dns-server)
  - [Set Rules](#set-rules)
  - [Enable Logs](#enable-logs)
- [sshguard](#sshguard)


## Install vmware tools 
[Open-VM-Tools](https://wiki.archlinux.org/index.php/VMware/Installing_Arch_as_a_guest#Open-VM-Tools)

[Xorg_configuration](https://wiki.archlinux.org/index.php/VMware/Installing_Arch_as_a_guest#Xorg_configuration)


    pacman -S open-vm-tools
    pacman -S xf86-video-vmware



<!--xf86-input-vmmouse, xf86-video-vmware, and mesa.
-->

    # open-vm-tools needs this
    cat /proc/version > /etc/arch-release 

### Service responsible for the Virtual Machine status report
    systemctl enable vmtoolsd
### Filesystem utility. Enables drag & drop functionality between host and guest through FUSE
    systemctl enable vmware-vmblock-fuse

    systemctl start vmtoolsd
    systemctl start vmware-vmblock-fuse

## For xorg
    systemctl start xf86-video-vmware

**Reboot**

## SSH
    useradd -m -g users -s /bin/bash marjorie
    passwd marjorie
    pacman -S openssh
    nano /etc/ssh/sshd_config
    DenyUsers  bob
    AllowUsers  marjorie
Start service

    systemctl start sshd

## Add user to sudo
    useradd -m -g users -G wheel,storage,power -s /bin/bash bob
    usermod -g wheel bob
    passwd bob


nano /etc/sudoers

    %wheel ALL=(ALL) ALL

## Tools

    pacman -S net-tools
    pacman -S dnsutils
    pacman -S firefox
    spm -S xxd
    spm -S openssl 



## Update your system
[Link to Mirrorlist](https://www.archlinux.org/mirrorlist/)

nano /etc/pacman.conf

    [...]
    [multilib]
    Include = /etc/pacman.d/mirrorlist
nano /etc/pacman.d/mirrorlist

    Server = https://archlinux.vi-di.fr/$repo/os/$arch
    Server = https://mirrors.arnoldthebat.co.uk/archlinux/$repo/os/$arch

### Update System
    pacman -Syu


## Iptables 

### Set policy to DROP
    iptables -P INPUT DROP
    iptables -P OUTPUT DROP
    iptables -P FORWARD DROP

### Set DNS server 
    echo nameserver 8.8.8.8 > /etc/resolv.conf

### Set Rules 
Allow DHCP

    iptables -A OUTPUT -p udp --sport 68 --dport 67 -j ACCEPT
    iptables -A INPUT -p udp --sport 67 --dport 68 -j ACCEPT
Allow SSH

    iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
    iptables -A OUTPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
Allow DNS

    iptables -A OUTPUT -d 8.8.8.8 -p udp --dport 53 -m state --state NEW,ESTABLISHED -j ACCEPT
    iptables -A INPUT -s 8.8.8.8 -p udp --sport 53 -m state --state ESTABLISHED -j ACCEPT
Allow HTTP

    iptables -A OUTPUT -p tcp --dport 80 -m state --state NEW,ESTABLISHED -j ACCEPT
    iptables -A INPUT -p tcp --sport 80 -m state --state ESTABLISHED -j ACCEPT
Allow HTTPS

    iptables -A OUTPUT -p tcp --dport 443 -m state --state NEW,ESTABLISHED -j ACCEPT
    iptables -A INPUT -p tcp --sport 443 -m state --state ESTABLISHED -j ACCEPT
Allow FTP

    iptables -A OUTPUT  -p tcp --dport 21 -m state --state NEW,ESTABLISHED -j ACCEPT
    iptables -A INPUT -p tcp --sport 21 -m state --state ESTABLISHED -j ACCEPT
Allow loopback

    iptables -A INPUT -i lo -j ACCEPT
    iptables -A OUTPUT -o lo -j ACCEPT

### Enable Logs

    iptables -A INPUT  -j LOG  -m limit --limit 12/min --log-level 4 --log-prefix 'IP INPUT drop: '
    iptables -A OUTPUT -j LOG  -m limit --limit 12/min --log-level 4 --log-prefix 'IP OUTPUT drop: '



## sshguard
    spm -S sshguard
    iptables -N sshguard
    iptables -A INPUT -m multiport -p tcp --destination-ports 21,22 -j sshguard
    iptables-save > /etc/iptables/iptables.rules
    systemctl enable sshguard 
    spm -S syslog-ng
    /usr/sbin/sshguard -l /var/log/auth.log -b /var/db/sshguard/blacklist.db
