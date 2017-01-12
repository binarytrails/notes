# 1. Preconfigure

Configure GRUB bootloader with encrypted root

    vim /etc/default/grub

Change this line to make it look like this

    GRUB_CMDLINE_LINUX="cryptdevice=/dev/sdx3:cryptroot"

[Read more](https://wiki.archlinux.org/index.php/Dm-crypt/System_configuration#cryptdevice)

And this line

    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"

> The presence of the word **splash** enables the splash screen with condensed text output. Adding the parameter **quiet** will only show the splash screen. In order to enable the *normal* text start up, you should remove both of these.

[Read more](https://wiki.archlinux.org/index.php/GRUB#Additional_arguments)

# 2. Install

## x86_64 with EFI

    modprobe dm-mod     # not always needed if not found move along

    grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch_grub --recheck --debug
    mkdir -p /boot/grub/locale
    cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo

## i386 without EFI

    grub-install --target=i386-pc --recheck /dev/sdx

# 3. Generate config

    grub-mkconfig -o /boot/grub/grub.cfg

[Read more](https://wiki.archlinux.org/index.php/MacBook#Installing_GRUB_to_EFI_partition_directly)

# 4. Verify

Find your devices UUID's

    blkid

Verify by looking for UUID of your cryptroot and root inside. These are simple commands that will be run on boot time. Try to understand the logic in order to find where to look.

    vim -R grub.cnf

