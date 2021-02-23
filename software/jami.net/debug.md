Follow the builds commands (/ring/builds.md) then:

# Daemon

    SIPLOGLEVEL=0 AVLOGLEVEL=0 RING_TLS_LOGLEVEL=0 gdb --args ./bin/dring -c -d -p

## Set a breakpoint

It's important to disable the shared library to use your local binary and if you do so then,
instead of generating a script at ```bin/dring``` it will be the true binary as the one at ```bin/.libs/dring```.

    cd build && .././autogen.sh
    .././configure --prefix=/usr --enable-debug --disable-shared
    gdb --args bin/.libs/dring -cd
    r ^Ctrl+C
    b <filename>.cpp:<line>

# Gnome client + LRC

    gdb --args ./gnome-ring

# Android client

    adb logcat *:D | grep `adb shell ps | egrep 'cx.ring' | cut -c10-15` > logring.txt

