# Reset trust

    rm -R /etc/pacman.d/gnupg
    pacman-key --init
    pacman-key --populate archlinux

# "Failed to commit transaction (invalid or corrupted package)" error

    find /var/cache/pacman/pkg/ -iname "*.part" -exec rm {} \;


# Installing package breaks depnedency

**exapmle:**

    :: installing xorgproto (2019.2-2) breaks dependency 'dmxproto' required by libdmx
    :: installing xorgproto (2019.2-2) breaks dependency 'xf86dgaproto' required by libxxf86dga

    # libdmx 1.1.4-1 depends on dmxproto which xorgproto 2019.2-2 does not provide
    pacman -Rdd  libdmx libxxf86dga
