# Reset trust

    rm -R /etc/pacman.d/gnupg
    pacman-key --init
    pacman-key --populate archlinux

# "Failed to commit transaction (invalid or corrupted package)" error

     find /var/cache/pacman/pkg/ -iname "*.part" -exec rm {} \;

