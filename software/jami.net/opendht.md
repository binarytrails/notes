# build

    # autotools
    ../configure --enable-static --disable-shared --disable-tools --disable-python --disable-doc --enable-proxy-server --enable-proxy-client --enable-push-notifications --prefix=/usr --no-create --no-recursion --enable-tests

    # cmake
    cmake -DOPENDHT_TESTS=ON -DOPENDHT_PROXY_SERVER=ON -DOPENDHT_PROXY_CLIENT=ON -DCMAKE_INSTALL_PREFIX=/usr -DOPENDHT_PROXY_SERVER_IDENTITY=ON -DOPENDHT_DOCUMENTATION=Off -DOPENDHT_PUSH_NOTIFICATIONS=ON -DCMAKE_BUILD_TYPE=Debug -DOPENDHT_PROXY_OPENSSL=On -DOPENDHT_PROXY_HTTP_PARSER_FORK=OFF -DOPENDHT_SANITIZE=On -DCMAKE_BUILD_TYPE=Debug ../

