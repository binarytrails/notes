# openssl

    # generate private key
    openssl genrsa -passout pass:x -out identity.key 4096
    
    # generate certificate request
    openssl req -new -key identity.key -out request.csr
    ...
    Common Name (e.g. server FQDN or YOUR name) []: 127.0.0.1
    ...

    # generate certificate
    openssl x509 -req -days 365 -in request.csr -signkey identity.key -out identity.crt

    # verify that your certificate and key match
    openssl x509 -noout -modulus -in identity.crt | openssl md5
    openssl rsa -noout -modulus -in identity.key | openssl md5

    # make a http request with the certificate
    curl -v https://127.0.0.1:8080 --cacert identity.crt

    # inspect certificate
    openssl x509 -in ca.crt -text -noout
    # inspect certificate revocation list
    openssl crl --noout -text -in crl.crl

    # extract certificate from server
    openssl s_client -showcerts -connect <ip:port>
