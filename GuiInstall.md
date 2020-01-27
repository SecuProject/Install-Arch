# INSTALL INTERFACE 

- [MATE](#mate)
  - [Window manager](#window-manager)
  - [(Desktop environment)](#desktop-environment)
  - [Display manager](#display-manager)
  - [to test](#to-test)
- [Budgie (if not MATE)](#budgie-if-not-mate)
  - [Window manager](#window-manager-1)
  - [(Desktop environment)](#desktop-environment-1)
  - [GNOME display manager](#gnome-display-manager)
  - [Thème](#th%c3%a8me)
  - [Source](#source)


# MATE

## Window manager
    pacman -S xorg xorg-server
## (Desktop environment)
    pacman -S mate mate-extra
## Display manager
Install 

    pacman -S lightdm
    pacman -S lightdm-gtk-greeter
    pacman -S lightdm-gtk-greeter-settings

nano /etc/lightdm/lightdm-gtk-greeter.conf

    # this should be the name of a directory under /usr/share/themes/
    # this should be the name of a fully featured icons set directory under /usr/share/icons/
    [greeter]                                            
    background = /usr/share/backgrounds/arch.png        
    theme-name = Arc-Dark
    icon-theme-name = Adwaita
    a11y-states = +keyboard;~reader
    position = 25%,center 50%,center

nano /etc/X11/xorg.conf.d/20-keyboard.conf

    Section "InputClass"
        Identifier "keyboard"
        MatchIsKeyboard "yes"
        Option "XkbLayout" "be"
        Option "XkbVariant" "nodeadkeys"
    EndSection
## to test
    dm-tool switch-to-greeter
    systemctl enable lightdm.service

# Budgie (if not MATE)
## Window manager
    pacman -S xorg xorg-server
## (Desktop environment)
    pacman -S budgie-desktop gnome
## GNOME display manager 
[Link Install Display Managers](https://wiki.manjaro.org/index.php/Install_Display_Managers)

    pacman -S gdm

    systemctl start gdm
    systemctl enable gdm
    reboot


## Thème 

    pacman -S arc-gtk-theme
    pacman -S papirus-icon-theme

## Source

https://www.mate-look.org/p/1197398/
