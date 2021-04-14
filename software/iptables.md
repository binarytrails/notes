# iptables

    # list loaded modules
    lsmod | grep -E "nf_|xt_|ip"

    # list available modules
    ls /lib/modules/$(uname -r)/kernel/net/netfilter
