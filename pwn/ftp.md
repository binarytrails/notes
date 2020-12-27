# ftp

## connect

    $ telnet <ip> 21
    USER anonymous
    PASS anonymous
    HELP

    $ ftp <ip>
    status
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
