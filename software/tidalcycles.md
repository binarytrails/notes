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

    # tidal with vim
    vim ~/.vimrc                # add line: Plug 'munshkr/vim-tidal'
    :PlugInstall
    :q

    cabal install tidal

# Run

    # ==> terminal 1 (sound server)
    su

    # generate DBUS_SESSION_BUS_ADDRESS and DBUS_SESSION_BUS_PID env variables
    # for jackd & sclang (supercollider) and apply (paste) them in bash
    dbus-launch --auto-syntax

    # needs sudo for real-time scheduling
    jackd -u -t 500 -d alsa -d hw:0 -r 44100 -p 1024

    # ==> terminal 2 (supercollider)

    # needs sudo to use same env variables as jackd
    sudo slang
    SuperDirt.start

# Interface

## tidalvim

    tidalvim

It will start a ```tmux``` session in which you have ```ctrl-b``` as CMD key.

    CMD-<arrow>         # switch between panels
    CMD-ctrl-<arrow>    # resize current pannel
    CMD :detach         # gtfo

### persisting session

Reported: https://github.com/munshkr/vim-tidal/issues/22

Solution:

    tmux kill-session -t tidal

