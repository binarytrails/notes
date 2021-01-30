# d-bus

## find services

    d-feet # graphical
    dbus-send --system --print-reply --dest=org.freedesktop.DBus  /org/freedesktop/DBus org.freedesktop.DBus.ListNames

## find methods

You can use ```--type=method_call``` to narrow things down.

    dbus-send --print-reply --dest=org.freedesktop.DBus /org/freedesktop/DBus org.freedesktop.DBus.Introspectable.Introspect

    dbus-send --system --print-reply --dest=org.freedesktop.NetworkManager \
        /org/freedesktop/NetworkManager/DnsManager org.freedesktop.DBus.Introspectable.Introspect
    dbus-send --system --print-reply --dest=org.freedesktop.NetworkManager \
        /org/freedesktop/NetworkManager/Settings org.freedesktop.DBus.Introspectable.Introspect

### python

    import dbus
    # system
    for service in dbus.SystemBus().list_names():
        print(service)
    # session
    for service in dbus.SessionBus().list_names():
        print(service)
