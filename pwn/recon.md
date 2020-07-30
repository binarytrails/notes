# passive

## identify

### dns

> SOA: Specifies authoritative information about a DNS zone, including the primary name server, the email of the domain administrator, the domain serial number, and several timers relating to refreshing the zone.

https://en.wikipedia.org/wiki/List_of_DNS_record_types

![ipv6-arch](https://en.wikipedia.org/wiki/IPv6#/media/File:IPv6_Prefix_Assignment_Example-en.svg)

#### dig

    $ dig google.com +short

[m]ail e[x]changer

    $ dig google.com MX +short

#### nslookup

    $ nslookup google.com

#### dnsrecon

    $ dnsrecon -t std -d google.com --json out.json

#### dnsdict6 (ipv6)

> Enumerates subdomains using a brute force search based on a supplied dictionary file or its own internal list.

    $ atk6-dnsdict6 google.com

#### dnsrevenum6

> Performs reverse DNS enumeration given an IPv6 address.

i.e. code.google.com. => 2607:f8b0:4004:810::200e

    $ atk6-dnsrevenum6 dns.google.com 2607:f8b0:4004::/48

## mapping route

### traceroute

    $ traceroute code.google.com

### hping3

> TCP/IP packet assembler and analyzer. TCP, UDP, ICMP, and raw-IP with ping-like interface

Normal ping to code.google.com is blocked but in a form of TCP SYN request (-S) to port 80 with 3 packets, it works!

    hping3 code.google.com -S -p 80 -c 3

### intrace

> InTrace is traceroute-like application that enables users to enumerate IP hops using existing TCP connections, both initiated from local network (local system) or from remote hosts. It could be useful for network reconnaissance and firewall bypassing.

    TTY1 $ intrace -h github.com

    TTY2 $ nc github.com 80
    GET / HTTP/1.0

