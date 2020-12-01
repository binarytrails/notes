# openvas

## authentication configuration not found

    # /var/log/openvas/openvasmd.log
    md   main:MESSAGE:2020-11-28 22h34.05 utc:1257: No SCAP database found
    md   main:MESSAGE:2020-11-28 22h34.05 utc:1257: No CERT database found
    lib auth:   INFO:2020-11-28 22h34.07 utc:1257: Authentication configuration not found.

    openvas-mkcert -f
    openvas-mkcert-client -i -n
    systemctl restart gsad

if you need to trust it, it's located at ```/var/lib/openvas/private/CA/cakey.pem```
