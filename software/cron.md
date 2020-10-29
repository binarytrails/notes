# cron

    # syntax:
    # min (0-59), hour (0-23), day of month (1-31), month (1-12), day of week (0-6), 0=sunday
    # redirect stderror to stdout
    0 3 * * * /bin/bash /home/me/script.sh 2>&1

    # location
    /var/spool/cron/crontabs/myjob

    # list
    crontab -l
