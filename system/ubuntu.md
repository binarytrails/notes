# ubuntu 24.04

## fix broken install

    sudo apt update
    sudo apt install -f 
    sudo dpkg --configure -a
    sudo apt upgrade
    sudo apt install --reinstall ubuntu-desktop
