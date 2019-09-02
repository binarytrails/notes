# aux to speakers

    pactl list short | egrep "alsa_(input|output)" | fgrep -v ".monitor"
    pacat -r-device=alsa_input.pci-0000_00_1f.3.analog-stereo | pacat -p --latency-msec=1
