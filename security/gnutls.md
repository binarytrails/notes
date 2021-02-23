# general

    # get node certificate information
    gnutls-cli 127.0.0.1 -p 8080

# ocsp (online certificate service protocol)

    # generate ocsp request
    ocsptool -q --load-cert=certificate.crt --load-issuer=rootca.crt --outfile ocsp-request.der

    # decode ocsp request
    cat ocsp-request.der | ocsptool --request-info

    # send ocsp request then, verify response
    ocsptool --ask http://127.0.0.1:8080 --load-cert=certificate.crt --load-issuer=rootca.crt --load-signer=rootca.crt
