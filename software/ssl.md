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

    # get certificate chain from server
    openssl s_client -connect google.com:443 -showcerts 2>&1 < /dev/null

# ocsp (online certificate status protocol)

    # get ocsp response if any
    openssl s_client -host wikipedia.com -port 443 -status < /dev/null

    # get server certificate
    openssl s_client -connect google.com:443 2>&1 < /dev/null | sed -n '/-----BEGIN/,/-----END/p' > google.pem

    # extract ocsp url
    openssl x509 -noout -ocsp_uri -in google.pem

    # validate certificate with chain through ocsp url
    openssl ocsp -issuer google-chain.pem -cert google.pem -text -url <ocsp-url>

## setup server

    # environment
    mkdir -p democa/newcerts
    touch democa/index.txt
    echo 01 > democa/serial

    # config
    sdiff /etc/ssl/openssl.conf ocsp.conf
    ...
                                      > [ v3_OCSP ]
                                      > basicConstraints = CA:FALSE
                                      > keyUsage = nonRepudiation, digitalSignature, keyEncipherment
                                      > extendedKeyUsage = OCSPSigning
                                      >
    [ usr_cert ]                        [ usr_cert ]
                                      > authorityInfoAccess = OCSP;URI:http://127.0.0.1:8080
    ...
    [ CA_default ]                      [ CA_default ]

    dir         = /etc/ssl              | dir         = democa        # Where everything is
    ...

    # ca keys
    openssl genrsa -out rootca.key 1024
    openssl req -new -x509 -days 3650 -key rootca.key -out rootca.crt -config  ocsp.cnf

    # client keys
    openssl genrsa -out certKey.key 1024
    openssl req -new -x509 -days 3650 -key certKey.key -out certificate.crt -config  ocsp.cnf
    openssl x509 -x509toreq -in certificate.crt -out request.csr -signkey certKey.key

    # sign with ca and include crl and ocsp urls in the certificate
    openssl ca -batch -startdate 150813080000Z -enddate 250813090000Z -keyfile rootca.key -cert rootca.crt -policy policy_anything -config ocsp.cnf -notext -out certificate.crt -infiles request.csr

    # ocsp keys
    openssl req -new -nodes -out ocspsigning.csr -keyout ocspsigning.key
    openssl ca -keyfile rootca.key -cert rootca.crt -in ocspsigning.csr -out ocspsigning.crt -config ocsp.cnf

    # run the server
    openssl ocsp -index democa/index.txt -port 8080 -rsigner ocspsigning.crt -rkey ocspsigning.key -CA rootca.crt -text

    # revoke certificate (requires server restart)
    openssl ca -keyfile rootca.key -cert rootCA.crt -revoke certificate.crt

    # make client request
    openssl ocsp -CAfile rootca.crt -issuer rootca.crt -cert certificate.crt -url http://127.0.0.1:8080 -resp_text
