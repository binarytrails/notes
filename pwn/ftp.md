# ftp

## connect

    $ telnet <ip> 21
    USER anonymous
    PASS anonymous
    HELP

    $ ftp <ip>
    help

## telnet native commands

    PORT 127,0,0,1,0,80 Indicate the ftp server to establish a connection with the IP 127.0.0.1 in port 80 (you need to put the 5th char as "0" and the 6th as the port in decimal or use the 5th and 6th to express the port in hex).
    EPRT |2|127.0.0.1|80| Indicate the ftp server to establish a TCP connection (indicated by "2") with the IP 127.0.0.1 in port 80. supports IPv6.
    LIST Send the list of files in current folder
    APPE /path/something.txt Indicate the ftp to store the data received from a passive connection or from a PORT/EPRT connection to a file. If the filename exists, it will append the data.
    STOR /path/something.txt Like APPE but it will overwrite the files
    STOU /path/something.txt Like APPE, but if exists it won't do anything.
    RETR /path/to/file A passive or a port connection must be establish. Then, the ftp server will send the indicated file through that connection
    REST 6 Indicate the server that next time it send something using RETR it should start in the 6th byte.
    TYPE i Set transfer to binary
    PASV Open a passive connection and will indicate the user were he can connects

## ftp binary commands

    status
    system
    site help
    get <file>
    put <local-file>
    rename <file> <file2>
    delete <file>
    help

## mount

    curlftpfs ftp://anonymous:anonymous@<ip>/ /mnt
    find /mnt -name <myfile>
    umount /mnt

## download files

    wget -m ftp://anonymous:anonymous@<ip>
    wget -m --no-passive ftp://anonymous:anonymous@<ip>

## nmap

    nmap -v -sV -n -p 21 --script=ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221 <ip>

## metasploit

### bruteforce ftp login

    Module options (auxiliary/scanner/ftp/ftp_login):

       Name              Current Setting                                 Required  Description
       ----              ---------------                                 --------  -----------
       BRUTEFORCE_SPEED  5                                               yes       How fast to bruteforce, from 0 to 5
       PASS_FILE         /tmp/pwds.txt                                   no        File containing passwords, one per line
       RHOSTS            <ip1> <ip2>                                     yes       The target host(s), range CIDR identifier, or 'file:<path>'
       RPORT             21                                              yes       The target port (TCP)
       STOP_ON_SUCCESS   true                                            yes       Stop guessing when a credential works for a host
       THREADS           10                                              yes       The number of concurrent threads (max one per host)
       USER_FILE         /tmp/users.txt                                  no        File containing usernames, one per line
       VERBOSE           true                                            yes       Whether to print output for all attempts
