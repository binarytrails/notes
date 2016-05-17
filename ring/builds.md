# Daemon

[How to build](https://tuleap.ring.cx/wiki/index.php?group_id=101&pagename=2.a-i+Build+Ring+daemon+on+Linux), 
[where to get](https://gerrit-ring.savoirfairelinux.com/#/admin/projects/ring-daemon)

    RING=$PWD/ring-daemon
    cd $RING/contrib

    mkdir build
    cd build
    ../bootstrap
    make
    # That's all !

    # Optionally, you may also type :
    make list

    # OR
    # make .packge?
    # to force using downloaded packages, not system locally installed.
    #make .gnutls .upnp ...
    # instead I had to:
    make .opendht
    
    cd $RING
    # Now generate autotools configuration files
    ./autogen.sh
    # Create Makefiles and config.h files for your build target
    ./configure --prefix=/usr
    # Build
    make
    # Install
    sudo make install

