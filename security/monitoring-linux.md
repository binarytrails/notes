# Monitoring

## Light-weight

### ss

    # log new connections excluding the unspecified address
    watch -n1 "ss -ltpa | grep -v 0.0.0.0 | tee -a ss.log"

    # monitor in live State, Peer Address:Port and Process
    watch -n1 "cat ss.log | grep -e ESTAB -e LISTEN | grep -v "*:*" | awk '{print \$1,\$5,\$6}' | sort | uniq"

### iptables

    # setup
    ## all tcp/udp connection with NEW state
    iptables -N LOGGING
    iptables -A INPUT -m state --state NEW -j LOGGING
    iptables -A LOGGING -j LOG --log-prefix "New incoming connection: " --log-level 4
    iptables -A LOGGING -j ACCEPT

    # watch in live
    watch tail -f /var/log/syslog | grep "New incoming connection: " | grep "DST=" /var/log/syslog | awk -F"DST=" '{print $2}' | awk '{prin t $1}' | sort | uniq

    # cleanup
    ## reboot if stuck
    iptables -D OUTPUT -j LOGGING
    iptables -F LOGGING
    iptables -X LOGGING
