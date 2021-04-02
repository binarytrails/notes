# Windows Firewall
    
    # block all in/outbound traffic
    netsh advfirewall set allprofiles firewallpolicy blockinbound,blockoutbound

    # remove all rules
    netsh advfirewall firewall delete rule all

    # allow basic outbound rules for ports 80,443,53,67,68
    netsh advfirewall firewall add rule name="Core Networking (HTTP-Out)" dir=out action=allow protocol=TCP remoteport=80
    netsh advfirewall firewall add rule name="Core Networking (HTTPS-Out)" dir=out action=allow protocol=TCP remoteport=443
    netsh advfirewall firewall add rule name="Core Networking (DNS-Out)" dir=out action=allow protocol=UDP remoteport=53 program="%%systemroot%%\system32\svchost.exe" service="dnscache"
    netsh advfirewall firewall add rule name="Core Networking (DHCP-Out)" dir=out action=allow protocol=UDP localport=68 remoteport=67 program="%%systemroot%%\system32\svchost.exe" service="dhcp"

    # allow inbound ports
    netsh advfirewall firewall add rule name="Web delivery" dir=in action=allow protocol=TCP localport=80

    # reset firewall in/outbound rules to default ones
    netsh advfirewall reset

You can always verify them by going Windows Defender Firewall > Advanced Settings.

Look at in/outbound rules and right click at root element > Properties to see the allow / block rules for different kind of networks.
