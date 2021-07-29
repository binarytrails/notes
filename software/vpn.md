# vpn

## no internet, can't resolve

First, make sure nothing is disabling a network interface (you would see No's):

    rfkill list

First thing to test is can you reach with a hard-typed dns server?

    $ ping 8.8.8.8 -c 1
    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=120 time=13.4 ms
    --- 8.8.8.8 ping statistics ---
    1 packets transmitted, 1 received, 0% packet loss, time 0ms

Here is our main issue that persistent across browsers:

    $ curl google.com
    curl: (6) Could not resolve host: google.com

This confirms us that some shady network interface is breaking the internet game for us:

    $ nslookup google.com 8.8.8.8
    Server:		8.8.8.8
    Address:	8.8.8.8#53

    Non-authoritative answer:
    Name:	google.com
    Address: 172.217.13.142
    Name:	google.com
    Address: 2607:f8b0:4020:806::200e

Now, if ping'ng google.com directly is unsuccessful, it might be a bogus network interface!

    $ ifconfig
    ipv6leakintrf0: flags=195<UP,BROADCAST,RUNNING,NOARP>  mtu 1500
    ...

This would be our bad boi. From quick [googling](https://www.google.com/search?hl=en&q=ipv6leakintrf0) it shows us there are many similar issues.

Let's destroy this annoyance:

    $ nmcli device show
    $ nmcli device delete ipv6leakintrf0
    Device 'ipv6leakintrf0' successfully removed.

If we check ifconfig it's gone as well as nmcli device, try again a ```curl google.com```, it should work.

However, we're not done here. It will persist after reboot. You will need to find what keeps loading it:

    $ grep -rni ipv6leakintrf0 /etc
    /etc/NetworkManager/system-connections/pvpn-ipv6leak-protection.nmconnection:5:interface-name=ipv6leakintrf0

Let's delete it to get rid of it forever:

    rm /etc/NetworkManager/system-connections/pvpn-ipv6leak-protection.nmconnection
