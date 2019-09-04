# openssl

    openssl genrsa -passout pass:x -out keypair.key 4096
    
    openssl req -new -key keypair.key -out request.csr
    
    openssl x509 -req -days 365 -in request.csr -signkey keypair.key -out cert.pem
