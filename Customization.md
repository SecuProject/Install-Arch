# Customization

## Grub Settings 

There are possibilities:
- Remove GRUB Menu 
- Install grub theme

### Remove GRUB Menu 

nano /etc/default/grub

    GRUB_DEFAULT=0
    GRUB_TIMEOUT=0
    GRUB_DISTRIBUTOR="Arch"
    GRUB_CMDLINE_LINUX_DEFAULT="quiet loglevel=3 rd.systemd.show_status=auto rd.udev.log_priority=3"
    GRUB_CMDLINE_LINUX=""

sudo grub-mkconfig -o /boot/grub/grub.cfg

### Install grub theme - Arch-Silence

> Download 

    git clone https://github.com/fghibellini/arch-silence.git /tmp/arch-silence


> Copy 

    sudo cp -TR /tmp/arch-silence/theme /boot/grub/themes/arch-silence

> Check 

    ls "/boot/grub/themes/arch-silence/theme.txt" -la


> Edit grub file 

sudo nano /etc/default/grub

    GRUB_THEME="/boot/grub/themes/arch-silence/theme.txt"

> Update grub

    sudo grub-mkconfig -o /boot/grub/grub.cfg

## Conky

### Install Conky

yay -S conky

> Default path 
> 
    (/usr/share/doc/conky-1.11.5_pre/conky.conf)

### Setup config file

mkdir -p ~/.config/conky

    wget https://github.com/SecuProject/Install-Arch/blob/master/conky.conf ~/.config/conky/conky.conf

> Run of test

conky --config="/home/bob/.config/conky/conky.conf"

> Auto start


nano /home/bob/.config/conky/startup.sh

    #/bin/bash
    killall conky
    sleep 5s && conky -c "$HOME/.config/conky/conky.conf" &


chmod 700 /home/bob/.config/conky/startup.sh

sudo ln -s /home/bob/.config/conky/startup.sh /etc/profile.d/conky.sh

## Suppress boot message 

sudo nano /etc/mkinitcpio.conf

    MODULES=(vmxnet3 vmw_vmci vmw_pvscsi vmw_balloon)
    HOOKS=(base udev block keyboard keymap filesystems)

sudo mkinitcpio -p /etc/mkinitcpio.d/linux.preset

## Test boot speed 

    systemd-analyze
    systemd-analyze blame
