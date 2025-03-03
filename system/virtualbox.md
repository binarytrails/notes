# VirtualBox

## Recovering VirtualBox after messing with folders

virtual machines

    cp -R /<old path>/VirtualBox VMs/ ~/

config

    cp /<old path>/.config/VirtualBox/VirtualBox.xml ~/.config/VirtualBox/

## Guest Additions would not work

Install the dependencies

    apt-get install -y dkms build-essential linux-headers-generic linux-headers-$(uname -r)

If it throws an error it means that in most of the cases that they are already inserted.
In the vbox machine, open a terminal and locate them. They should be at [sr0].

    lsblk

If they are mounted at [/dev/cdrom]

    umount /dev/cdrom

Afterwards, whenever they were mounted or not

    mkdir /dev/vboxguests
    mount /dev/sr0 /dev/vboxguests
    ls -l /dev/vboxguests

They should be mounted as [read-only] and you should only be able to execute them as a normal user with sudoer powers.

    sudo ./VBoxLinuxAdditions.run
    reboot

## Boot from usb

You must boot virtualbox as root to have access to [sdX] or play with privileges.

    VBoxManage internalcommands createrawvmdk -filename usb_sdc.vmdk -rawdisk /dev/sdc

## Mount VDI as ISO

    VBoxManage clonehd --format RAW ubuntu.vdi ubuntu.img
    mount -t ext3 -o loop,rw ./ubuntu.img /mnt

## Install Guess Additions

    $ pacman -S virtualbox-guest-iso
    $ pacman -Ql virtualbox-guest-iso
    virtualbox-guest-iso /usr/lib/virtualbox/additions/VBoxGuestAdditions.iso

or these modern days:
    
    Devices menu > Insert Guest Addition CD Image.. > Download

## Usb not detected

1. Install the extension pack
2. Add your user to vboxusers

    groups roger
    adduser roger vboxusers

## Kernel driver not installed (rc=-1908)

    apt-get install dkms
    /etc/init.d/vboxdrv setup

## Resize screen dimentions by hand

    vboxmanage setextradata "roger-vm" "CustomVideoMode1" "1366x768x32"

## Unable to insert the virtual optical disk VERR_PDM_MEDIA_LOCKED

    sudo su
    cd /media
    mkdir cdrom
    mount /dev/cdrom /media/cdrom

## Start tag expected, '<' not found

    cd ~/VirtualBox\ VMs/<name>
    cp <name>.vbox <name>-broken-1.vbox
    cp <name>.vbox-prev <name>.vbox

## Menu bar disappeared

It means you're in scale mode, to get out:

    Host + c

## Linux Shared Folder: Permission denied

    sudo usermod -aG vboxsf $(whoami)
    # logout/login
