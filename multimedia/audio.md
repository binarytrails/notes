# audio

    # find sound card
    lspci -v | grep -A7 -i "audio"

    # use legacy HDA-Intel driver
    $ cat /etc/modprobe.d/audio-fix.conf
    blacklist snd-sof-pci
    options snd-intel-dspcfg dsp_driver=1

## alsa

    # find sinks / hardware devices
    aplay -lL

## pulse audio

    pulseaudio --kill
    pulseaudio --start -vvv

    systemctl --user stop pulseaudio.socket
    systemctl --user stop pulseaudio.service
    systemctl --user start pulseaudio.socket
    systemctl --user start pulseaudio.service

    # useful prior to changing to alsa to avoid interfernces
    systemctl --user mask pulseaudio.socket
    systemctl --user unmask pulseaudio.socket

    # service has to run
    pacmd list-sink-inputs

    pacmd set-sink-input-mute <index> false         # unmute
    pacmd set-sink-input-volume <index> 0x10000     # set volume to 100%

    # aux to speakers
    pactl list short | egrep "alsa_(input|output)" | fgrep -v ".monitor"
    pacat -r-device=alsa_input.pci-0000_00_1f.3.analog-stereo | pacat -p --latency-msec=1

    # it's also interactive
    $ pacmd
    Welcome to PulseAudio 14.2! Use "help" for usage information.
    >>> help
    Available commands:
        help                      Show this help
        list-modules              List loaded modules
        list-cards                List cards
        list-sinks                List loaded sinks
        list-sources              List loaded sources
        ...
