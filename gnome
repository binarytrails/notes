# Touchpad tap
To see what is set:

    synclient

The keys: TapButton1 (one finger), TapButton2 (two fingers), TapButton3 (three fingers) have values: left-click (1), middle-click (2) and right-click (3).

Inverse actions done with two to three fingers:

    synclient TapButton2=3
    synclient TapButton3=2

# Gnome gdm login background
The only one that worked for me on Arch Linux:

    sudo -u gdm dbus-launch gsettings set org.gnome.desktop.screensaver picture-uri 'file:///usr/share/backgrounds/gnome/picture.jpg'
Then, you can verify it worked:

    sudo -u gdm gsettings get org.gnome.desktop.screensaver picture-uri

Or verify using dconf:

    sudo -u gdm dconf read /org/gnome/desktop/screensaver/picture-uri

