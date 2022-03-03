# rbash

> restricted bash

    # enabling for a user
    cd /bin
    ln -s bash rbash
    useradd -m ruser
    usermod --shell /bin/rbash ruser
    grep -rni ruser /etc/passwd

    # start directly
    bash -r
    cd /etc/
    -rbash: cd: restricted
