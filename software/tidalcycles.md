# Install

    $ uname -a
    Linux Host 4.13.7-1-ARCH #1 SMP PREEMPT Sat Oct 14 20:13:26 CEST 2017 x86_64 GNU/Linux

    # install packer manually from AUR (makepkg -s PKGFILE; pacman -U <packer>)

    # install
    pacman -S ghc                   # 8.2.1-2
    pacman -S ghc-static            # 8.2.1-3
    pacman -S jack2                 # 1.9.10-6
    pacman -S supercollider         # 3.8.0-2
    packer -S sc3-plugins-git       # 3.8.0.r41.g5342a4a-1
    packer -S vim-plug              # 0.7.2-1

    # as root
    sclang                      # start supercollider client
    include("SuperDirt")        # install it
    ctrl-D                      # exit

    # point from extensions to installed ones
    cd /root/.local/share/SuperCollider/downloaded-quarks/
    ln -s /root/.local/share/SuperCollider/downloaded-quarks/Vowel/
    ln -s /root/.local/share/SuperCollider/downloaded-quarks/Dirt-Samples/
    ln -s /root/.local/share/SuperCollider/downloaded-quarks/quarks/

    # tidal with vim
    vim ~/.vimrc                # Plug 'munshkr/vim-tidal'
    :PlugInstall
    :q

    cabal install tidal

# Run

    # ==> terminal 1 (sound server)

    su
    dbus-launch --auto-syntax
    jackd -u -t 500 -d alsa -d hw:0 -r 44100 -p 1024

    # ==> terminal 2 (supercollider)

    slang
    SuperDirt.start

    # ==> terminal 3 (vim as tidal-cycles editor)

    tidalvim                    # start

# Destroy

## tidalvim: persisting session

To be re-encountered for clean solution:
https://github.com/munshkr/vim-tidal/issues/22

