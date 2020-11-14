# kernel

## attr

### set read only

chmod won't work:

    $ sudo chattr -V +i /etc/resolv.conf
    ----i---------e----- /etc/resolv.conf

    $ sudo chattr -V -i /etc/resolv.conf
    Flags of /etc/resolv.conf set as --------------e-----

    $ lsattr /etc/resolv.conf
    --------------e----- /etc/resolv.conf
