# hardware

## get model

    dmidecode | grep Version | sed -n '2p'
    dmidecode | grep 'SKU Number' | head -1
    dmidecode | grep -A 9 "System Information"
