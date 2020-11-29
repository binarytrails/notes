# elevate linux

## gather info

What's the OS? What version? What architecture?

    cat /etc/*-release
    uname -i
    lsb_release -a (Debian based OSs)

Who are we? Where are we?

    id
    pwd

Who uses the box? What users? (And which ones have a valid shell)

    cat /etc/passwd
    grep -vE "nologin|false" /etc/passwd

What's currently running on the box? What active network services are there?

    ps aux
    netstat -antup
    ss -ltpa

What's installed? What kernel is being used?

    dpkg -l (Debian based OSs)
    rpm -qa (CentOS / openSUSE )
    uname -a
