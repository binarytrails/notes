Follow the builds commands (/ring/builds.md) then:

# Daemon

    SIPLOGLEVEL=0 AVLOGLEVEL=0 RING_TLS_LOGLEVEL=0 gdb --args ./bin/dring -c -d -p

# Gnome client + LRC

    gdb --args ./gnome-ring

# Android client

    adb logcat *:D | grep `adb shell ps | egrep 'cx.ring' | cut -c10-15` > logring.txt

