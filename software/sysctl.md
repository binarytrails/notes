# sysctl

## generic

    # reload from /etc/sysctl.conf 
    sysctl -p

## disable connectivity checks

> Note that your distribution might set /proc/sys/net/ipv4/conf/*/rp_filter to strict filtering. That works badly with per-device connectivity checking, which uses SO_BINDDEVICE to send requests on all devices. A strict rp_filter setting will reject any response and the connectivity check on all but the best route will fail.

    sysctl -w net.ipv4.conf.all.rp_filter=0
    sysctl -w net.ipv4.conf.default.rp_filter=0
    sysctl -w net.ipv4.conf.wlp58s0.rp_filter=0
