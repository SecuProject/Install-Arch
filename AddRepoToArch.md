<h1> Add repo to Arch </h1>

- [Add Arch Strike repo](#add-arch-strike-repo)
  - [Add key](#add-key)
  - [Install tools](#install-tools)
  - [Check repo](#check-repo)
- [Add Black Arch repo](#add-black-arch-repo)
- [Install yay](#install-yay)



## Add Arch Strike repo 

 uncom => multilib 

sudo nano /etc/pacman.conf

    [archstrike]
    Server = https://mirror.archstrike.org/$arch/$repo


> Update

    sudo pacman -Syy
### Add key 

    sudo pacman -S wget
    sudo pacman-key --init
    sudo dirmngr < /dev/null
    sudo pacman-key -r 9D5F1C051D146843CDA4858BDE64825E7CBC0D51
    sudo pacman-key --lsign-key 9D5F1C051D146843CDA4858BDE64825E7CBC0D51

### Install tools 

sudo pacman -Syy


    sudo pacman -S archstrike-keyring
    sudo pacman -S archstrike-mirrorlist


sudo nano /etc/pacman.conf

    [archstrike]
    Include = /etc/pacman.d/archstrike-mirrorlist


sudo pacman -Syy


### Check repo 

    pacman -Sl archstrike
    pacman -Sg | grep archstrike
    pacman -Sgg | grep archstrike-<groupname>

## Add Black Arch repo 

    sudo pacman -Syyu
    sudo curl -O https://blackarch.org/strap.sh
    sudo chmod +x strap.sh
    sha1sum strap.sh
[Check the Hash](https://blackarch.org/downloads.html#install-repo)

    sudo ./strap.sh



## Install yay
Require git 

    sudo pacman -S git
Clone repo 

    cd /tmp
    git clone https://aur.archlinux.org/yay.git
    cd yay
Install 

    makepkg -si
    
# System upgrade 
    sudo pacman -Syu

## sync the Pacman database
    sudo pacman -Syy


## Update pgpKey 
    sudo pacman-key --refresh-keys
