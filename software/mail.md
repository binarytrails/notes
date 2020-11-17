# mail

## postfix

    postfix start
    postfix status

    postconf -v -o message_size_limit=0 | grep message_size_limit
    /etc/postfix/main.cf | grep message_size_limit 
    postconf -v | grep message_size_limit

    postfix stop
    update-rc.d postfix disable

## send an email to a file

    echo "postmaster: root" >> /etc/aliases
    newaliases
    mailx -s Test root
    mail
