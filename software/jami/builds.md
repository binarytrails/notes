[All build information](http://dl.ring.cx/docs/compiling_and_installing/index.html)

# Daemon

[How to build](https://tuleap.ring.cx/wiki/index.php?group_id=101&pagename=2.a-i+Build+Ring+daemon+on+Linux),
[where to get](https://gerrit-ring.savoirfairelinux.com/#/admin/projects/ring-daemon)

    cd contrib && mkdir build
    cd build && ../bootstrap
    make list                   # list contribs
    make                        # everything

    make .opendht .gnutls ...   # individually download & compile tarballs locally

    # pjproject is special because it selects statically compiled lib if already available
    rm -rf pjproject/ ../x86_64-pc-linux-gnu/include/pj* ../x86_64-pc-linux-gnu/lib/libpj* -rf
    make .pjproject

    cd ring-daemon && mkdir build
    cd build && .././autogen.sh                 # now generate autotools configuration files
    ./configure --prefix=/usr --disable-shared  # to be able to load in gdb: --disable-shared
    make
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

# Jami-Project

Checkout your gerrit patch set within a submodule of a higher entity:

    cd .git/modules/daemon/
    curl -Lo /home/n0t/jami/ring-project/.git/modules/daemon/hooks/commit-msg \
        https://review.jami.net/tools/hooks/commit-msg; \
        chmod +x /home/n0t/jami/ring-project/.git/modules/daemon/hooks/commit-msg
    git fetch https://review.jami.net/ring-daemon refs/changes/59/11959/16 && git checkout FETCH_HEAD
    git pull origin refs/changes/59/11959/16

## Build Android on Arch Linux

    export JAVA_HOME=/usr/lib/jvm/default/jre
    export ANDROID_HOME=/opt/android-sdk
    export ANDROID_SDK=/opt/android-sdk
    export ANDROID_NDK_ROOT=/opt/android-ndk
    export ANDROID_NDK=/opt/android-ndk
    export PATH=$PATH:$PATH:$ANDROID_HOME/tools:$ANDROID_NDK:$ANDROID_NDK:$JAVA_HOME/bin

    ./make-ring.py --install --distribution=Android
    ANDROID_ABI=armeabi-v7a arm64-v8a ./make-ring.py --install --distribution=Android
