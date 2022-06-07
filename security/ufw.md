# ufw

    #Install UFW
    sudo apt-get install ufw

    #Use UFW to Manage Firewall Rules. Set Default Rules
    sudo ufw default allow outgoing
    sudo ufw default deny incoming

    #Add Rules. Rules can be added in two ways: By denoting the port number or by using the service name. For example, to allow both incoming and outgoing connections on port 22 for SSH.

    #You can run:
    sudo ufw allow ssh

    #You can also run:
    sudo ufw allow 22

    #Similarly, to deny traffic on a certain port (in this example, 231) you would only have to run:
    sudo ufw deny 231

    #To further fine-tune your rules, you can also allow packets based on TCP or UDP. The following will allow TCP packets on port 80:
    sudo ufw allow 80/tcp
    sudo ufw allow http/tcp

    #Whereas this will allow UDP packets on 1725:
    sudo ufw allow 1725/udp

    #Advanced Rules
    To allow connections from an IP address:
    sudo ufw allow from 204.61.107.0

    To allow connections from a specific subnet:
    sudo ufw allow from 204.61.107.0/24

    To allow a specific IP address/port combination:
    sudo ufw allow from 204.61.107.0 to any port 22 proto tcp

    #Remove Rules

    To remove a rule, add delete before the rule implementation. If you no longer wished to allow HTTP traffic, you could run:
    sudo ufw delete allow 80

    Deleting also allows the use of service names.

    #Edit UFW’s Configuration Files
    /etc/default/ufw
    /etc/ufw/before.rules

    #Check UFW Status
    sudo ufw status

    #Enable the Firewall
    sudo ufw enable

    #Disable the Firewall
    sudo ufw disable

    #Logging. You can enable logging with the command:
    sudo ufw logging on

    #Check log file
    /var/logs/ufw
