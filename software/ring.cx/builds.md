[All build information](http://dl.ring.cx/docs/compiling_and_installing/index.html)

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

# LRC

[How to build](https://tuleap.ring.cx/wiki/index.php?pagename=Build%20LibRingClient%20%28LRC%29&group_id=101),
[where to get](https://gerrit-ring.savoirfairelinux.com/#/admin/projects/ring-lrc)

Build in debug mode:

    cd $LIBRINGLIENT
    mkdir build
    cd build
    cmake .. -DRING_BUILD_DIR=$RING/src -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Debug
    make
    sudo make install

# Gnome client

[How to build](https://tuleap.ring.cx/wiki/index.php?group_id=101&pagename=Build+Gnome+Client+for+Ring),
[where to get](https://gerrit-ring.savoirfairelinux.com/#/admin/projects/ring-client-gnome)

Build with linking to your LRC (to be able to debug it):

    mkdir build
    cd build
    cmake -DLibRingClient_PROJECT_DIR= [add_your_LRC_path_project_folder] ..
    make
    sudo make install

