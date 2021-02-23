# openvas

## change admin password

    sudo openvasmd --user=admin --new-password=tototata

## debug logs

      4  Errors.
      8  Critical situation.
     16  Warnings.
     32  Messages.
     64  Information.
    128  Debug.  (Lots of output.)

    # /etc/openvas/gsad_log.conf
    # /etc/openvas/openvasmd_log.conf
    change 127 -> 128

    openvas-stop
    openvas-start

    # check /var/log/openvas/*.log

## vagrantfile

### debian buster

    Vagrant.configure("2") do |config|
      config.vm.box = "debian/buster64"
      config.vm.network "forwarded_port", guest: 9392, host: 9392
      config.vm.network "forwarded_port", guest: 9390, host: 9390
    end

## install

### debian buster

    apt-get update && sudo apt-get upgrade --yes
    apt-get install openvas
    apt install rsync sqlite3 xsltproc -f
    openvas-setup
    cd /lib/systemd/system
    # change 127.0.0.1 but 0.0.0.0
    vim greenbone-security-assistant.service openvas-manager.service openvas-scanner.service
    systemctl daemon-reload
    openvas-stop && openvas-start

## issues

### authentication configuration not found

    # /var/log/openvas/openvasmd.log
    md   main:MESSAGE:2020-11-28 22h34.05 utc:1257: No SCAP database found
    md   main:MESSAGE:2020-11-28 22h34.05 utc:1257: No CERT database found
    lib auth:   INFO:2020-11-28 22h34.07 utc:1257: Authentication configuration not found.

    openvas-mkcert -f
    openvas-mkcert-client -i -n
    systemctl restart gsad

if you need to trust it, it's located at ```/var/lib/openvas/private/CA/cakey.pem```

### 503 Service temporarily down

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

### ERROR: No OpenVAS SCAP database found. (Tried: /var/lib/openvas/scap-data/scap.db)

Unfortunetely, feeds of openvas are down forever.

    $ sudo openvas-scapdata-sync
    [i] This script synchronizes a SCAP data directory with the OpenVAS one.
    [i] This script is for the SQLite3 backend.
    [i] SCAP dir: /var/lib/openvas/scap-data
    [i] Will use rsync
    [i] Using rsync: /bin/rsync
    [i] Configured SCAP data rsync feed: rsync://feed.openvas.org:/scap-data
    rsync: failed to connect to feed.openvas.org (89.146.224.58): Connection timed out (110)
    rsync: failed to connect to feed.openvas.org (2a01:130:2000:127::d1): Network is unreachable (101)
    rsync error: error in socket IO (code 10) at clientserver.c(122) [Receiver=3.0.9]
    [e] Error: rsync failed. Your SCAP data might be broken now.
