# record screen in gnome-terminal

    gnome-terminal -x sh -c 'ffmpeg -f x11grab  -s 1366x768 -i :0.0 -r 25 -vcodec libx264 \
        `date +%Y-%m-%d_%H-%M-%S`_BTEVCpcampFormBook.mkv; exec bash'

# speedup

    # x60 faster
    ffmpeg -i input.mp4 -filter:v "setpts=PTS/60" output.mp4
