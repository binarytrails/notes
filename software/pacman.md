# Reset trust

    rm -R /etc/pacman.d/gnupg
    pacman-key --init
    pacman-key --populate archlinux
