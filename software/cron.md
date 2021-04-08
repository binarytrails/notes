# cron

    crontab -l      # list
    crontab -e      # edit

## cronfile

    # syntax:
    # min (0-59), hour (0-23), day (1-31), month (1-12), day of week (0-6) with 0=sunday
    # redirect stderror to stdout (2>&1)
    0 3 * * * /bin/bash /home/me/script.sh 2>&1

    # make sure env is loaded or you'll get 'command not found'
    # how to load it: export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    30 2 * * 1 /bin/bash echo toto >> /var/log/toto.log 2>&1

## places

    /var/spool/cron/crontabs/myjob     # location
    /var/log/syslog                    # logs changes
