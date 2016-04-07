# Find excluding directories

Find at / (path1 OR path2) if found dir return True & don't descend (-prune) OR you_data_to_find (rest).

    find / \( -path /sys -o -path /var \) -prune -o -type d -name site-packages

# Command output as file
http://www.gnu.org/software/bash/manual/bash.html#Process-Substitution

    diff <(echo "abc") <(echo "abd") 
    
# Sha1sum check

    echo "hash filename" | sha1sum -c
    
# Converting with ffmpeg & avconv
    
    ffmpeg -fflags +genpts -i input.avi -map_metadata 0:s:0 -codec copy -f mp4 output.mp4   # removes metadata
    avconv -i in.webm -c:v libx264 -crf 20 -c:a aac -strict out.mp4

# Download files from url

    wget -r -P <destination> -A jpeg,jpg,bmp,gif,png,webm,webp http://www.simonstalenhag.se/

# Get open window info

    xprop -root | grep window\ id
    xprop -id [id]

# Apply afile attributes to bfile

    touch -r afile bfile

# Run-level a.k.a. boot manager

    sysv-rc-conf

# What is going on?

[top] is a great tool but a little fade on colors and interactivity...

    htop

# Disk space

Filesystems disk space usage

    df -h

Estimate directories space usage in [h]uman & [s]ummarized

    du -hs

# Ip information

    traceroute www.google.com
    whois www.google.com | less

# Network statistics

Network statistics of upload & download rates

    vnstat -i wlan0

# Mount ssh directory

[mount]

    sshfs roger@ipaddr:/home/toto /mnt/roger/

[umount]

    fusermount -u mountpoint
    
# Music on Console (moc)

Copy and edit the configuration

    cd ~/.moc && cp /usr/share/doc/moc/examples/config.example.gz ./ && gunzip config.example.gz && mv config.example config
    mocp -T transparent-background

# Dictionnary

Download dictionnaries at [abloz.com](http://abloz.com/huzheng/stardict-dic/) or [web.archive.org](https://web.archive.org/web/20140917131745/http://abloz.com/huzheng/stardict-dic/dict.org/) they are all in the [DICT](https://en.wikipedia.org/wiki/DICT) format.

    mkdir -p /usr/share/stardict/dic/
    tar -xvjf downloaded_dictionnary.tar.bz2 -C /usr/share/stardict/dic
    tar -xvzf downlaoded_dictionnary.tar.gz -C /usr/share/stardict/dic
    sdcv word

# Midnight Commander

Text base file navigator

    mc -b

# Dynamic Virtual Terminal Manager

[vim] output can get messy but hey, give it a try!

    dvtm

Here are some commands to help you start up

    ctrl+G      Mod command, you type it before every below command

    Mod-c       Create a new shell window. Ex: (ctrl+G, c)
    Mod-x       Close focused window.
    Mod-j       Focus next window.
    Mod-k       Focus previous window.
    Mod-Space   Toggle between defined layouts (affects all windows).
    Mod-t       Change to vertical stack tiling layout.
    MOD+h       Enlarged master area size.
    MOD-l       Reduce master area size.
    Mod-g       Change to grid layout.
    Mod-q       Quit dvtm.

(More commands)[http://www.brain-dump.org/projects/dvtm/]