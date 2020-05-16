# Customization

## Install grub theme - Arch-Silence

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