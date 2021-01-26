# Install

1. Enable USB debugging

    1. Go into your device ```Settings > About``` and click many times on your build number
    2. Go into ```Settings > Developer Tools``` and activate USB debug or more reboot options

2. Go into bootloader

        sudo adb reboot bootloader

3. Install a custom recovery

    I would suggest: https://twrp.me/Devices/

        fastboot devices

    If nothing show up look how to setup udev rules and find device id with ```lsusb```.

        fastboot flash recovery recovery.img

    Try selecting ```Recovery``` and if it works skip the section 4:

4. Unlock cellphone

    It's locked by the company to hold you by their waranty agreement. However, you are stronger than this or you don't care.

        # get the ecrypted data
        fastboot oem get_unlock_data

        # concat the encoded data & paste it on your cellphone company site to get the unlock key

        # unlock with the key XYZ from the website
        fastboot oem unlock XYZ

5. Wipe & install from Recovery

    ```Wipe > Advance Wipe```

    Everything beside the usb or if you're cautious only ```Cache, System and Data```.

        adb push lineageos.zip open_gapps.zip /sdcard/

    Install by selecting all zips and hitting install.

    Reboot & pray.

# Update

Rebooted into TWRP: manually install: ```/data/data/org.lineageos.updater/app_updates/lineage-*.zip```

# Anomalies

## Can't write on sdcard

It also says "missing external sdcard" when you have an internal one that was perfectly working before..

After stimulating hours, I realized that this issue is not documented at all. It is due to the incorrect permissions on your folder and you can solve it by:

    # enable root for adb in dev options
    adb root
    adb shell
    
    chown -R media_rw:media_rw /data/media/
    find /data/media/ -type d -exec chmod 775 {} ';'
    find /data/media/ -type f -exec chmod 664 {} ';'

    # if the previous doesn't work then,
    restorecon -FR /data/media/
