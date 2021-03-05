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

# user

    # rename
    usermod --login new_username old_username

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

# conditions

    # check if command exists
    command=`which mycmd`
    if [ -z "$command" ]; then
        echo "Command not found"
    fi

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

# formating

    # make a patch
    diff -Naur file1 file2 > file.patch

    # lines to commas
    cat data.txt | paste -s -d, -

    # remove head and tail banners
    cat ssh.key | tail -n +2 | head -n -1

    # Command output as file
    diff <(echo "abc") <(echo "abd") 

# functions

    # counter
    echo 0 > /tmp/c; cat /tmp/c
    for i in seq 3 ; do
        i=$(cat /tmp/c) && echo $((i+1)) > /tmp/c && echo $(cat /tmp/c);
    done;

# network

    # ip information
    traceroute www.google.com
    whois www.google.com | less

    # network statistics
    Network statistics of upload & download rates
    vnstat -i wlan0

    # mount ssh directory
    sshfs roger@ipaddr:/home/toto /mnt/roger/
    fusermount -u mountpoint # unmount

# multimedia

    # converting with ffmpeg & avconv
    ffmpeg -fflags +genpts -i input.avi -map_metadata 0:s:0 -codec copy -f mp4 output.mp4   # removes metadata
    avconv -i in.webm -c:v libx264 -crf 20 -c:a aac -strict out.mp4

    # download files from url
    wget -r -P <destination> -A jpeg,jpg,bmp,gif,png,webm,webp

    # get open window info
    xprop -root | grep <window id>
    xprop -id <id>

# literacy

    # dictionnary
    mkdir -p /usr/share/stardict/dic/
    tar -xvjf downloaded_dictionnary.tar.bz2 -C /usr/share/stardict/dic
    tar -xvzf downlaoded_dictionnary.tar.gz -C /usr/share/stardict/dic
    sdcv word

- http://abloz.com/huzheng/stardict-dic/
- https://web.archive.org/web/20140917131745/
- http://abloz.com/huzheng/stardict-dic/dict.org/
