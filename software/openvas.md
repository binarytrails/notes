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

## 503 Service temporarily down

    # issue when trying to scan
    Operation: Start Task
    Status code: 503
    Status message: Service temporarily down

    # diagnosis
    $ sudo openvasmd --get-scanners
    <UUID>  OpenVAS Default
    $ sudo openvasmd --verify-scanner <UUID>
    Failed to verify scanner.

    # fix
    $ cd /var/lib/openvas/ && ls
    CA  cert-data  gnupg  mgr  plugins  private  scap-data
    sudo openvasmd --modify-scanner <UUID> --scanner-ca-pub CA/cacert.pem \
        --scanner-key-pub CA/clientcert.pem --scanner-key-priv private/CA/clientkey.pem
    Scanner modified.
    sudo openvasmd --verify-scanner <UUID>
    Scanner version: OTP/2.0.

