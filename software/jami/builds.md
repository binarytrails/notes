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

## Building Ring with ASan

Building with ASan is easy and allows you to catch some interesting issues from time to time

More fsanitize options: https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html

    export CFLAGS="-O0 -g -fsanitize=address"
    export LDFLAGS="-fsanitize=address"
    export CXXFLAGS="$CFLAGS"
    ./configure

Then, build as usual.

Running autogen.sh should not be necessary.

In order to build with ASan, you will need GCC >= 4.8, but, anyways, not sure you can build Ring with GCC 4.7 :)

    CXXFLAGS="-fsanitize=address" ./configure --enable-debug

This is enough to build the daemon with ASan (but not to build the contribs with ASan, which is useful to debug PJSIP stuff)

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

