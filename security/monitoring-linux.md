# Monitoring

## Light-weight

### iptables

    # setup
    ## all tcp/udp connection with NEW state
    iptables -N LOGGING
    iptables -A INPUT -m state --state NEW -j LOGGING
    iptables -A LOGGING -j LOG --log-prefix "New incoming connection: " --log-level 4
    iptables -A LOGGING -j ACCEPT

    # watch in live
    watch tail -f /var/log/syslog | grep "New incoming connecttion: " | grep "DST=" /var/log/syslog | awk -F"DST=" '{print $2}' | awk '{prin t $1}' | sort | uniq

    # cleanup
    ## reboot if stuck
    iptables -D OUTPUT -j LOGGING
    iptables -F LOGGING
    iptables -X LOGGING
