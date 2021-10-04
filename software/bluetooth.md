# bluetooth

## arch-linux

    pacman -S bluez bluez-utils gnome-bluetooth
    systemctl start bluetooth

    bluetoothctl
    agent KeyboardOnly
    default-agent
    
    power on
    scan on

    pair <device-mac>
    connect <device-mac>

    quit
