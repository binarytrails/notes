# openssl

    # generate private key
    openssl genrsa -passout pass:x -out keypair.key 4096
    
    # generate certificate request
    openssl req -new -key keypair.key -out request.csr
    ...
    Common Name (e.g. server FQDN or YOUR name) []: 127.0.0.1
    ...

    # generate certificate
    openssl x509 -req -days 365 -in request.csr -signkey keypair.key -out cert.pem

    # make a http request with the certificate
    curl -v https://127.0.0.1:8080 --cacert cert.pem

    # inspect certificate
    openssl x509 -in ca.pem -text -noout

    # extract certificate from server
    openssl s_client -showcerts -connect <ip:port>
