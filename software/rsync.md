# backup/update the entire system from root /

    n0t System $ cat excludes
    /dev/*
    /proc/*
    /sys/*
    /tmp/*
    /run/*
    /mnt/*
    /media/*
    /lost+found
    /home/*/.thumbnails/*
    /home/*/.cache/*
    /home/*/.local/share/Trash/*
    # temp for speedup
    *.vmdk

    n0t System $ rsync -aAXv --update --delete --exclude-from 'excludes' / $(echo "$(pwd)/mysys/")

