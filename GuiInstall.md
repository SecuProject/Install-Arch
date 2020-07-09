# INSTALL INTERFACE 

- [INSTALL INTERFACE](#install-interface)
  - [Window manager](#window-manager)
  - [Desktop environment](#desktop-environment)
  - [Display manager](#display-manager)
    - [lightdm-webkit](#lightdm-webkit)
    - [lightdm-gtk](#lightdm-gtk)
  - [Cursor](#cursor)
  - [Source](#source)


## Window manager
    pacman -S xorg xorg-server

nano /etc/X11/xorg.conf.d/10-monitor.conf

    Section "Monitor"
        Identifier "DisplayPort-0"
        Modeline "1920x1080X"  325.08  1920 1944 1976 2056  1080 1083 1088 1098 +hsync +vsync
        Option "PreferredMode" "1920x1080X"
    EndSection

    Section "Screen"
        Identifier "Screen0"
        Monitor "DisplayPort-0"
        DefaultDepth 24
        SubSection "Display"
            Modes "1920x1080X"
        EndSubSection
    EndSection

nano /etc/X11/xorg.conf.d/20-keyboard.conf

    Section "InputDevice"
        Identifier     "Mouse0"
        Driver         "mouse"
        Option         "Protocol" "auto"
        Option         "Device" "/dev/psaux"
        Option         "Emulate3Buttons" "no"
        Option         "ZAxisMapping" "4 5"
    EndSection

    Section "InputDevice"
        Identifier     "Keyboard0"
        Driver         "kbd"
    EndSection

    Section "InputClass"
        Identifier "keyboard"
        MatchIsKeyboard "yes"
        Option "XkbLayout" "be"
        Option "XkbVariant" "nodeadkeys"
    EndSection

## Desktop environment
    pacman -S mate mate-extra

> Theme 

    pacman -S arc-gtk-theme
    pacman -S papirus-icon-theme

## Display manager
Install lightdm

    pacman -S lightdm
    
For the greeter menu there are two possibilities:
- lightdm-gtk
- lightdm-webkit

### lightdm-webkit
    pacman -S lightdm-webkit2-greeter
    pacman -S lightdm-webkit-theme-litarvan
    
nano /etc/lightdm/lightdm-webkit2-greeter.conf
    
    [greeter]
    debug_mode          = false
    detect_theme_errors = true
    screensaver_timeout = 300
    secure_mode         = true
    time_format         = LT
    time_language       = auto
    webkit_theme        = litarvan

nano /etc/lightdm/lightdm.conf

    greeter-session=lightdm-webkit2-greeter

### lightdm-gtk
    
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

nano /etc/lightdm/lightdm.conf

    greeter-session=lightdm-gtk-greeter

> To test the config
> 
    dm-tool switch-to-greeter
    systemctl enable lightdm.service


## Cursor

    https://www.mate-look.org/p/1197398/


## Source 

    https://subscription.packtpub.com/book/networking_and_servers/9781849519724/1/ch01lvl1sec17/configuring-gui-using-xorg-should-know