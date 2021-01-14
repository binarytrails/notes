# kali

## enable shared folder in live

1. make sure you boot it with usb persistence
2. once booted, add a transient folder in vbox (don't check automount)
3. mkdir ~/share && sudo mount -t vboxsf <host-folder-name> ~/share/
