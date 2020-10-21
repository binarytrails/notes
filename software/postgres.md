# debian

## update to new version

    psql --version
    pg_lsclusters
    systemctl stop postgresql
    apt-get install -y postgresql-13 postgresql-server-dev-13
    systemctl daemon-reload
    pg_createcluster 13 main --start
    systemctl start postgresql@13-main
    psql --version
    netstat -ltpn

## msf missing role

    msf5 > db_connect msfdb init
    [-] Error while running command db_connect: Failed to connect to the Postgres data service: FATAL:  role "<user>" does not exist

    sudo -u postgres createuser -s $(whoami); createdb $(whoami)
