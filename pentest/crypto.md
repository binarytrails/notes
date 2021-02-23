# crypto

## conversions

    echo "toto" | hexdump
    hexdump -C file.bin
    
    xxd -b file.bin
    xxd file.hex
    xxd -p file                 # plain/continious hex
    xxd -p file | tr -d '\n'    # oneline hex
