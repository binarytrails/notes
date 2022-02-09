# ssh

    # no matching key exchange method found. Their offer: diffie-hellman-group1-sha1
    ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 ...

## authorized_keys

    chmod 700 .ssh
    chmod 600 .ssh/authorized_keys      # add one public key per line

    ssh-copy-id user@host               # copy public key to remote authorized_keys

## formats

    ssh-keygen -t rsa           # -----BEGIN OPENSSH PRIVATE KEY----- which new format
    ssh-keygen -t rsa -m PEM    # -----BEGIN RSA PRIVATE KEY----- which is old format

    # convert new format to old format (will overwrite private key passed to -f and set empty pwd)
    ssh-keygen -p -N "" -m pem -f /path/to/key

## local forwading

build the tunnel

    -f      Requests ssh to go to background just before command execution.
           This is useful if ssh is going to ask for passwords or passphrases,
           but the user wants it in the background.  This implies -n.
           The recommended way to start X11 programs at a remote site
    
    -N     Do not execute a remote command.  This is useful for just forwarding ports.

    ssh user@mordor.net -fN -L 6666:127.0.0.1:22 

    # with embedded ssh inside of a command it may look like:
    ./command --args -- -fN -L127.0.0.1:6666:127.0.0.1:22

some -L args variants:

    local_port:remote_address:remote_port 
    local_address:local_port:remote_address:remote_port

use the tunnel

    # make sure the user matches the public key in ~/.ssh/authorized_keys on remote server
    ssh user@localhost -p 6666  -i ~/.ssh/private-key uname -a

## gather data and run

    ssh host > host.log << EOF
    help
    pwd
    uname -a
    ps auxf
    EOF
