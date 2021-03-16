# gnome-shell

## touchpad tap

To see what is set:

    synclient

The keys: TapButton1 (one finger), TapButton2 (two fingers), TapButton3 (three fingers) have values: left-click (1), middle-click (2) and right-click (3).

Inverse actions done with two to three fingers:

    synclient TapButton2=3
    synclient TapButton3=2

## gdm login background

The only one that worked for me on Arch Linux:

    sudo -u gdm dbus-launch gsettings set org.gnome.desktop.screensaver picture-uri 'file:///usr/share/backgrounds/gnome/picture.jpg'
Then, you can verify it worked:

    sudo -u gdm gsettings get org.gnome.desktop.screensaver picture-uri

Or verify using dconf:

    sudo -u gdm dconf read /org/gnome/desktop/screensaver/picture-uri

## gnome-terminal profiles

* Backup current

    dconf dump /org/gnome/terminal/ > org.gnome.terminal

* Nuke current

    dconf reset -f /org/gnome/terminal/

* Load yours

    dconf load /org/gnome/terminal/ < org.gnome.terminal

## getting rid of tracker

> Tracker is a filesystem indexer, metadata storage system and search tool

    systemctl --user mask tracker-store.service tracker-miner-fs.service tracker-miner-rss.service \
        tracker-miner-extract.service tracker-miner-apps.service tracker-writeback.service
    tracker reset --hard
    killall tracker-miner-fs-3

## NetworkManager

### mac randomization during wi-fi scanning is crashing the connection

This is a very weird bug. I have two laptops using ArchLinux with Gnome Shell both are using same packages versions. One could connect to the University network and the another one couldn't. After investigation, I found that something was failing related to MAC address as seen in below log. Turns out that desactivating MAC boundcing during a connection solved my issue.

**Logs:**

    $ journalctl -rxt NetworkManager
    
    Sep 15 13:52:22 Host NetworkManager[416]: <info>  [1473961942.1820] device (wlp66s6): set-hw-addr: set MAC address to AA:AA:AA:AA:AA:AA (
    Sep 15 13:46:37 Host NetworkManager[416]: <info>  [1473961597.1519] device (wlp66s6): set-hw-addr: set MAC address to BB:BB:BB:BB:BB:BB (
    Sep 15 13:46:36 Host NetworkManager[416]: <info>  [1473961596.6442] device (wlp66s6): state change: failed -> disconnected (reason 'none'
    Sep 15 13:46:36 Host NetworkManager[416]: <warn>  [1473961596.6401] device (wlp66s6): Activation: failed for connection 'Univers
    Sep 15 13:46:36 Host NetworkManager[416]: <info>  [1473961596.6386] manager: NetworkManager state is now DISCONNECTED
    Sep 15 13:46:36 Host NetworkManager[416]: <info>  [1473961596.6379] device (wlp66s6): state change: prepare -> failed (reason 'none') [40
    Sep 15 13:46:36 Host NetworkManager[416]: <warn>  [1473961596.6378] device (wlp66s6): set-hw-addr: new MAC address CC:CC:CC:CC:CC:CC not 
    Sep 15 13:46:36 Host NetworkManager[416]: <info>  [1473961596.1310] manager: NetworkManager state is now CONNECTING
    Sep 15 13:46:36 Host NetworkManager[416]: <info>  [1473961596.1304] device (wlp66s6): state change: disconnected -> prepare (reason 'none
    Sep 15 13:46:36 Host NetworkManager[416]: <info>  [1473961596.1299] audit: op="connection-activate" uuid="11111111-1111-1111-1111-1111111
    Sep 15 13:46:36 Host NetworkManager[416]: <info>  [1473961596.1282] device (wlp66s6): Activation: starting connection 'Universit

**Solution:**

    $ cat /etc/NetworkManager/NetworkManager.conf 
    
    [device]
    wifi.scan-rand-mac-address=no

    [connection]
    wifi.cloned-mac-address=stable

This configuration is demonstrated in the [ArchWiki](https://wiki.archlinux.org/index.php/NetworkManager#Configuring_MAC_Address_Randomization) and it is explained in depth on [gnome blog post](https://blogs.gnome.org/thaller/2016/08/26/mac-address-spoofing-in-networkmanager-1-4-0/).
