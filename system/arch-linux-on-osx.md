# Archlinux on MacBook
*29 December 2014* 

> What MacBook? MacBook Pro mid-2011 with Intel CPU & Graphics.

> What ArchLinux? ArchLinux x86_64.


# What are my motivations?

1. Help curious minds to get into something delightful.
2. ArchLinux is one level deeper diving into Linux.
3. MacBook laptops are the most aesthetic in the 2010s market.


# Results

![img](/static/frontend/img/articles/Arch Linux On Macbook.jpg)


# Guide Flow

I am assuming that you are aware of what you are doing. Hence, there is no one to blame but yourself for the destruction of your system.

You should have had read or have the [ArchLinux MacBook wiki page](https://wiki.archlinux.org/index.php/MacBook) open in a separate tab.

Take this as an example and a proof of concept.

The ArchLinux wiki is tremendous and the community on freenode servers is great. But remember...

> [The ArchLinux Mentality](/static/frontend/img/svg/Arch Linux Help Guide.svg)


# Table of content

1. [Preparations](#preparations)

2. [Installing the base system](#installing)

3. [Configuring with finess using Gnome 3](#configuring)

4. [Problem Solving](#problem-solving)

5. [Learn More](#learnmore)


<a name="preparations"></a>
# Preparations

## Creating a bootable USB key

Partition: GPT

Filesystem Type: FAT

    dd bs=4M if=arch.iso of=/dev/sdx
    sync # let it finish

## Partitionning

We will use the **GPT-UEFI** model that is opposed to the MBR-BIOS one. 

*If you are not running a computer with EFI this partitionning model is not relevent to you.*

[Read More](https://wiki.archlinux.org/index.php/Arch_boot_process#Firmware_types)

1. Determine the device.

        root@archiso ~ # lsblk

2. Erase all of your partitions using cgdisk.

    X is the letter of your device. I will use this to define your letter, but don’t assume your drive is sdx. It isn’t, most likely. To find your device, write the last command and use your logic.

        cgdisk /dev/sdx

3. Follow the simple partitions model below and adapt it to your needs.

    Gdisk, fdisk, cgdisk & parted **(NOT cfdisk)** adds the first starting offset to the physical sector size reported by the disk in order to align it properly. Basically, the starting offset is just there to guarantee that the first partition and the rest start on a proper boundary. You can use + or % instead of G, Mb in older tools to avoid losing alignment. In our case, we will use cgdisk.

    [Read more](http://www.ibm.com/developerworks/library/l-linux-on-4kb-sector-disks/index.html)

    We will encrypt our root and home partitions. We won't use swap space for the hibernation.

    <table>
        <tr>
            <th>Partition</th>
            <th>Size</th>
            <th>Type</th>
            <th>Name</th>
        </tr>
        <tr>
            <td>sdx1</td>
            <td>200MB</td>
            <td>ef00</td>
            <td>/boot/efi</td>
        </tr>
        <tr>
            <td>sdx2</td>
            <td>100MB</td>
            <td>8300</td>
            <td>/boot</td>
        </tr>
        <tr>
            <td>sdx3</td>
            <td>50G</td>
            <td>8300</td>
            <td>root</td>
        </tr>
        <tr>
            <td>sdx4</td>
            <td>Rest</td>
            <td>8300</td>
            <td>home</td>
        </tr>
    </table>

    If you are worrying about the root partition size, you must understand that your heavy personal files will be located at your /home partition. Thus, they won't be directly on your root. It is a good idea to use a separate home partition in order to protect your files during the maintenance of your system or in a scenario of migration to a different distribution. That, I might say, is a very plausible scenario.

        > df -h
        Filesystem             Size  Used Avail Use% Mounted on
        /dev/mapper/cryptroot   50G   15G   32G  32% /

    After 7 months on Arch Linux and a whole bunch on installs, only 15G were used.

        # cgdisk example
        First sector (2048, default = ...): [Enter]
        Size in sectors or {KMGTP} (default = ...): [Size]
        Current type is 8300 (Linux filesystem)
        Hex code or GUID (L to show codes, Enter = 8300): [Type] 
        Current partition name is ''
        Enter new partition name, or  to use the current name: [Name]

    You can always consult the cgdisk [walkthrough](http://www.rodsbooks.com/gdisk/cgdisk-walkthrough.html) to be sure.

    Afterwards, [ verify ] and [ write ] and you are done partitioning.

4. Formatting the partitions

        mkfs.vfat /dev/sdx1
        mkfs.ext2 /dev/sdx2

    [Read more](https://help.ubuntu.com/community/LinuxFilesystemsExplained)

    Let's set up the dm-crypt (luks) on the entire root sdx3 without lvm.

        cryptsetup -y -v luksFormat /dev/sdx3
        cryptsetup open /dev/sdx3 cryptroot
        mkfs.ext4 /dev/mapper/cryptroot
        mount /dev/mapper/cryptroot /mnt

    Do the same with your home partition but mount it to /home.

    > The boot flag is not required with a GPT-UEFI model.


<a name="installing"></a>
## Installing the base system

As was previously done, the cryptroot is mounted at /mnt. Let's create directories to allow the pacstrap to populate our partitions by using the same directory names as in the /mnt.

    mkdir /mnt/boot
    mkdir /mnt/boot/efi
    
    mount /dev/sdx2 /mnt/boot
    mount /dev/sdx1 /mnt/boot/efi

1. Installing the base system

        pacstrap /mnt base base-devel

    There is also a more bare-metal called ‘base-devel’ but you will be missing, nano, mkinitcpio and other useful commands.

    **DO NOT** mount your system elsewhere than at /mnt. Pacstrap works with /mnt folder. Otherwise you will get something really weird.

2. Identifying partitions

        genfstab -p /mnt >> /mnt/etc/fstab

    [Read more](https://wiki.archlinux.org/index.php/Fstab#Identifying_filesystems)

3. Connecting to the system

    It’s time to chroot to configure the system we created. Chrooting allows you to connect to the mounted system and make the main process and all its subprocesses to believe that this directory is the root. Thus, we can install everything we need to the system via USB key.

        arch-chroot /mnt /bin/bash

    Arch-chroot is a wrapper of chroot and by default the command without /bin/bash will point to /bin/sh which, in many Linux systems like Arch, will be pointing back to /bin/bash. Still, it’s a bad habit because in the modern systems it won’t always be the case.

    > One doesn’t need to restart from the beginning if something goes wrong from this point. Take some time to **think**, to **read**, **ask some questions** on IRC freenode channels and think again.

4. Localtime

        ln -s /usr/share/zoneinfo/Canada/Eastern /etc/localtime

5. Language

    > Locales are used by glibc and other locale-aware programs or libraries for rendering text, correctly displaying regional monetary values, time and date formats, alphabetic idiosyncrasies, and other locale-specific standards. - ArchWiki

    Open locale.gen and uncomment the two rows for your keyboard. i.e. en_US...

        nano /etc/locale.gen

    Create the /etc/locale.conf file substituting your chosen locale manually or by issuing

        echo LANG=en_US.UTF-8 > /etc/locale.conf

    Generate your locale configuration

        locale-gen

6. Keyboard

    By default, the keyboard is in English and keys mapping is fine.

    [Language](https://wiki.archlinux.org/index.php/Beginners%27_guide#Change_the_language)
    [Font & Keymap](https://wiki.archlinux.org/index.php/Beginners%27_guide#Console_font_and_keymap)

7. Hostname

        echo "Torvalds" >> /etc/hostname

8. Getting a network connection

    You can use ethernet cable and enable dhcp.

        systemctl start dhcpcd.service
        ping 8.8.8.8

    If you want to mainly use the ethernet, you should enable the service.

        systemctl enable dhcpcd.service

    Otherwise, stop the dhcpcd.service if running.

        systemctl stop dhcpcd.service

    And, use the old fashion **wifi-menu** for example.

        wifi-menu

9. Pacman Time! No gaming allowed.

    Let's download a fresh copy of the master package list from the server and force-refresh all packages even if they appear up to date.

        pacman -Syy

    Search for regexp in package database

        pacman -Ss [package]

    Get the package information

        pacman -Si [package]

    Install a package

        pacman -S [package]

    [Read more](https://www.archlinux.org/pacman/pacman.8.html)

    I prefer using [vim] instead of [nano]. It is up to you. Efibootmgr is essential for GRUB, that we will use later, & net-tools have ifconfig and other useful commands.

        pacman -S efibootmgr net-tools vim

    > The pkgfile searches the .files metadata created by repo-add(8) to retrieve file information about packages. By default, the provided target is considered to be a filename and pkgfile will return the package(s) which contain this file.

        pacman -S pkgfile; pkgfile -u

    For instance, you could find

        root@arch ~ # pkgfile -s ifconfig
        core/net-tools


10. Configure the early user space enviroment a.k.a. ramdisk.

        pacman -S mkinitcpio linux
        vim /etc/mkinitcpio.conf

    Insert into *HOOKS* between the words *encrypt* and *filesystems* where *encypt* is before *filesystems*.

        HOOKS="...encrypt...filesystems..."

    Initiate ramdisk enviroment

        mkinitcpio -p linux

    [Read more](https://wiki.archlinux.org/index.php/Mkinitcpio)

11. Configure GRUB bootloader with encrypted root

        pacman -S grub
        vim /etc/default/grub

    Change this line to make it look like this

        GRUB_CMDLINE_LINUX="cryptdevice=/dev/sdx3:cryptroot"

    [Read more](https://wiki.archlinux.org/index.php/Dm-crypt/System_configuration#cryptdevice)

    And this line

        GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"

    > The presence of the word **splash** enables the splash screen with condensed text output. Adding the parameter **quiet** will only show the splash screen. In order to enable the *normal* text start up, you should remove both of these.

    [Read more](https://wiki.archlinux.org/index.php/GRUB#Additional_arguments)

    Install the bootloader

        modprobe dm-mod     # not always needed if not found move along
        
        grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch_grub --recheck --debug
        mkdir -p /boot/grub/locale
        cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo

    Generate a configuration 

        grub-mkconfig -o /boot/grub/grub.cfg

    [Read more](https://wiki.archlinux.org/index.php/MacBook#Installing_GRUB_to_EFI_partition_directly)

    Find your devices UUID's

        blkid

    Verify by looking for UUID of your cryptroot and root inside. These are simple commands that will be run on boot time. Try to understand the logic in order to find where to look.
        
        vim -R grub.cnf

12. Configure encrypted home with crypttab

    > The /etc/crypttab (or, encrypted device table) file contains a list of encrypted devices that are to be unlocked when the system boots, similar to fstab. This file can be used for automatically mounting encrypted swap devices or secondary filesystems. It is read before fstab, so that dm-crypt containers can be unlocked before the filesystem inside is mounted. Note that crypttab is read after the system has booted, so it is not a replacement for unlocking via mkinitcpio hooks and boot loader options in the case of an encrypted root scenario. - [ArchWiki](https://wiki.archlinux.org/index.php/Dm-crypt/System_configuration#crypttab).

    Find the encrypted home partition UUID.

        > blkid
        ...
        /dev/sda4: UUID="00000a00-a0a0-0000-aa00-0aaa0a00aaaa" TYPE="crypto_LUKS" PARTLABEL="life" PARTUUID="..."
        /dev/mapper/crypthome: UUID="..." TYPE="ext4"
        ...

    Copy the /dev/sdx4 UUID **not the /dev/mapper one** and paste it to the /etc/crypttab file.

        > cat /etc/crypttab
        # <name>       <device>                                     <password>              <options>
        crypthome      UUID=00000a00-a0a0-0000-aa00-0aaa0a00aaaa    none                    luks,timeout=120

    Finally, update the fstab to mount it automatically during the boot as follows:

        > cat /etc/fstab
        # <file system>			<mount point>   <type>  	<options>       	<dump>  <pass>
        ...
        /dev/mapper/crypthome		/home		ext4		defaults		    0       2
        ...

13. Last notes

    If you're planning to use **wifi-menu** instead of the ethernet cable for further installations after booting into your system, you should install its dependencies right away.

        pacman -S dialog wpa_supplicant iwd

    Now you can safely exit the chroot, unmount your partitions and reboot into your new fresh system.

        exit
        umount /mnt/boot/efi
        umount /mnt/boot
        umount /mnt
        reboot

    Cross your fingers and hold tight, we hope you will enjoy your flight!


<a name="configuring"></a>
## Configuring with finess using Gnome 3

Welcome to your new system!

In this section, I will help you get started with a basic **G**raphical **U**ser **I**nterface and a basic system configuration using Gnome 3.

1. Get your MacBook Arch drivers

    Graphic card
    
        lspci | grep VGA
        pacman -S xf86-video-intel

    Touchpad

        pacman -S xf86-input-synaptics

    Everything else worked out of the box. Otherwise, go to the [post-installation](https://wiki.archlinux.org/index.php/MacBook#Post-installation) section.

        reboot

2. Get Gnome 3

    Living in Bash interpreter is o.k. but it can get a little depressing during the lazy days.

    > GNOME (pronounced /ɡˈnoʊm/ or /ˈnoʊm/) Shell is the graphical shell of the GNOME desktop environment starting with version 3 - Wikipedia

    I suggest you install the **gnome** group instead of *gnome-extra* and you should definetly learn the purpose of each [package](https://www.archlinux.org/groups/x86_64/gnome/) inside. You will be aware of what will be running in your system and it could even save some of your precious ressources on a daily basis. You can always install more of the packages later using pacman.

        pacman -S gnome

    Start Gnome

        systemctl start gdm

    You should be logged in as root, if not then write **root** as username and hit enter.

    [Read More](https://wiki.archlinux.org/index.php/Gnome#Installation)

3. Create & Configure a New User

    Go into *Settings* > *Users* and create a new administrator user.

    To allow this user to run commands we will use the [sudoers](https://wiki.archlinux.org/index.php/Sudo) powers.

            pacman -S sudo

    To open this file you have do the following with the editor of your choice. Mine is vim.

            EDITOR=vim visudo

    Add the last line to this section

            ##
            ## User privilege specification
            ##
            root ALL=(ALL) ALL
            new_awesome_username ALL=(ALL) ALL

    Logout and login as the user that you have created.

4. Configuring the WIFI using AUR & package build

    **You should definitly read the [AUR wiki page](https://wiki.archlinux.org/index.php/Arch_User_Repository).**

    Install this group package to be able to makepkg.

        pacman -S --needed base-devel

    I succeeded with [broadcom-wl](https://aur.archlinux.org/packages/broadcom-wl/) that is in AUR. For other WIFI drivers -> [Read More](https://wiki.archlinux.org/index.php/MacBook#Wi-Fi).

        mkdir ~/Builds && cd ~/Builds

    Under Package Actions > Download tarball , save it to ~/Builds.

        mkdir broadcom-wl
        tar xvf broadcom-wl.tar.gz broadcom-wl/

    Read the PKGBUILD file carefully because shell commands will be executed from it.

        cd broadcom-wl/ && vim -R PKGBUILD

    Makepkg and **-s** will resolve all of its dependecies.

        makepkg -s PKGBUILD

    Install the driver.

        pacman -U broadcom-wl.tar.xz

    Unload other conflicting modules.

        rmmod b43
        rmmod ssd

    Load the wl module.

        modprobe wl
        systemctl enable NetworkManager.service

    Make sure to have the **networkmanager** package installed & to enable **N**etwork**M**anager.service. It is case sensitive!

    If you have **dhcpcd.service** running it will cause a disconnection after association with an access point on wifi.

    [Read More](https://wiki.archlinux.org/index.php/Broadcom_BCM4312#Loading_the_wl_kernel_module)

5. Add transparency to windows

    Devilspie is THE tool to use. It works with S-expressions.

    > Devil's Pie can be configured to detect windows as they are created, and match the window to a set of rules. If the window matches the rules, it can perform a series of actions on that window.

        mkdir ~/.devilspie/
        touch ~/.devilspie/gnome-terminal.ds

    Here is an example of **gnome-terminal.ds** to lower the gnome terminal opacity.

        (if
            (is (application_name) "Terminal")
            (begin
                (opacity 85)
            )
        )

    The command below will show you the results with the current window.

        devilspie
        ctrl+C

    You can use it to see the proprieties of the open windows.

    And please remember...

    > A totally crack-ridden program for freaks and weirdos who want precise control over what windows do when they appear - [Gnome-Wiki](https://wiki.gnome.org/Projects/DevilsPie)

    Funny guys!


<a name="problem-solving"></a>
## Problem Solving

Wait. Something went really wrong and you need help?

        echo "Spiderman Days" | curl -F 'sprunge=<-' http://sprunge.us

Get your command output and go to #archlinux on irc.freenode.net and believe in humanbeing goodwill.

### Tweak Tool: Error on Global Dark Theme

A tool to customize advanced GNOME 3 options.

Error On: Enabling Global Dark Theme
Solution:

        touch ~/.config/gtk-3.0/settings.ini

### eog: Can't open jpeg photos

Get the codecs

        pacman -S imlib libjpeg-turbo jasper mjpegtools openjpeg

### modprobe: FATAL: Module wl not found.

**After update to kernel: 3.18.2-2**

1. Download the [patch](https://gist.githubusercontent.com/hobarrera/ac0e6225210ac5bb13f6/raw/763797294307fe1cc754edd9e81b6174dc0535d4/broadcom-sta-6.30.223.248-linux-3.18-null-pointer-crash.patch) in your PKGBUILD directory.

2. Edit your PKGBUILD file for add the new patch:

    1. Line 15: add the new patch you just download in the "source" list: 'null-pointer-crash.patch'
    2. Line 34: add the new patch in prepare function: "patch -p1 -i null-pointer-crash.patch"

3. Make package

        makepkg -f -A --skipinteg

Solution from the broadcom-wl AUR [page](https://aur.archlinux.org/packages/broadcom-wl/) by acssilva.


<a name="learnmore"></a>
## Learn More

### MBR VS GPT

The Master Boot Record (MBR) was developed during the 80’s.

GUID Partition Table (GPT) was developed during the 90’s.
</br>
MBR disks only have one partition table to keep track of all the blocks in the partition and is limited to three primary partitions and one extended partition.

GPT can have many partition tables and is limited to 128 partitions per disk.
</br>
MBR partitions are being limited to 2TB (terabytes) in size. 

GPT partitions are being limited to 18 EB (Exabyte’s) or 1 million terabytes.

<p class="footer">Goodbye dear deer.</p>
