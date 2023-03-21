# Reset trust

    rm -R /etc/pacman.d/gnupg
    pacman-key --init
    pacman-key --populate archlinux
    pacman-key --populate blackarch

# error: key "XXXX" could not be looked up remotely

    sudo pacman -Sy archlinux-keyring

# "Failed to commit transaction (invalid or corrupted package)" error

    find /var/cache/pacman/pkg/ -iname "*.part" -exec rm {} \;


# Installing package breaks depnedency

**exapmle:**

    :: installing xorgproto (2019.2-2) breaks dependency 'dmxproto' required by libdmx
    :: installing xorgproto (2019.2-2) breaks dependency 'xf86dgaproto' required by libxxf86dga

    # libdmx 1.1.4-1 depends on dmxproto which xorgproto 2019.2-2 does not provide
    pacman -Rdd  libdmx libxxf86dga

# Rollback

    # go to the the pacman packages cache
    cd /var/cache/pacman/pkg

    # remove echo or copy paste the generated command
    cat /var/log/pacman.log | grep upgraded | sed 's/(//g' | sed 's/)//g' | \
        awk '{print $1" "$4"-"$5" "$6" "$4"-"$7}' | grep "2021-07-26T10" | \
        awk '{print $2"-x86_64.pkg.tar.zst"}' | xargs echo pacman -U
