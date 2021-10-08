# dns

    # find the dns server
    host -t ns <domain-name>

    # find computer in ad
    nslookup <computer>.<domain>.LOCAL <dns-ip>

    # ldap lookup with dc domain
    nslookup
    set q=srv
    _ldap._tcp.dc._msdcs.<domain>

    # show route / add route to resolve it / precise network interface / delete it
    route -n
    ip route add XX.YY.ZZ.0/24 via <gateway-ip>
    ip route add XX.YY.ZZ.0/24 via <gateway-ip> dev <network-interface>
    ip route delete XX.YY.ZZ.0/24

## systemd resolver

    systemctl stop systemd-resolved

    # don't edit those just backup and link to auto generated one
    mv /etc/resolv.conf /etc/resolv.conf.orig
    ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf

    cp /etc/systemd/resolved.conf /etc/systemd/resolved.conf.orig
    echo "
    [Resolve]
    DNS=8.8.8.8 8.8.4.4
    FallbackDNS=1.1.1.1 1.0.0.1 2001:4860:4860::8888 2001:4860:4860::8844 2606:4700:4700::1111 2606:4700:4700::1001

    # method 1: TLS only way
    DNSSEC=true
    DNSOverTLS=true

    # method 2: TLS with fallback way
    #DNSSEC=allow-downgrade
    #DNSOverTLS=opportunistic

    # activate cache if you're afraid your DNS servers will go down or to speed up
    #Cache=yes
    " > /etc/systemd/resolved.conf

    # in network setting overwrite your router wifi one by google (2) and cloudflare (2)
    # this would prevent a fallback allowing your ISP snooping i.e.:
    #  Using degraded feature set ... instead of TLS+EDNS0+D0 for DNS server <router-ip>
    8.8.8.8,8.8.4.4,1.1.1.1,1.0.0.1

    # don't forget our beloved ipv6
    2001:4860:4860::8888,2001:4860:4860::8844,2606:4700:4700::1111,2606:4700:4700::1001

    # activate on start and right away or just restart
    systemctl enable systemd-resolved --now

    # optional: for advance debug on every transaction in journalctl do:
    # automatically reloaded on a successful save and exit
    systemctl edit systemd-resolved.service
    # add these lines at beggining:
    [Service]
    Environment=SYSTEMD_LOG_LEVEL=debug

    # check how thigs are going in terms of DNS server and the security protocols used in live
    journalctl -f -u systemd-resolved

    # check which interface used which DNS server
    resolvectl status
