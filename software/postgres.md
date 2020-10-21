# debian

## update to new version

    psql --version
    pg_lsclusters
    Ver Cluster Port Status Owner    Data directory              Log file
    12  main    5432 online postgres /var/lib/postgresql/12/main /var/log/postgresql/postgresql-12-main.log

    systemctl stop postgresql
    apt-get install -y postgresql-13 postgresql-server-dev-13
    systemctl daemon-reload
    systemctl start postgresql@13-main

    pg_createcluster 13 main --start
    # if the software using it needs only 13+ postgres
    pg_dropcluster 12 main --stop

    psql --version
    netstat -ltpn

## msf missing role

    msf5 > db_connect msfdb init
    [-] Error while running command db_connect: Failed to connect to the Postgres data service: FATAL:  role "<user>" does not exist

    sudo -u postgres createuser -s $(whoami); createdb $(whoami)
