# centos

## release

    cat /etc/redhat-release

## rpm

    # find package files
    rpm -ql <name>

## roller coaster

### lastlog is gigantic

Jump to navigationJump to search

Why does the /var/log/lastlog file appear so huge on 64-bit machines? You might do a directory listing and see something like this:

    # ls -l /var/log/lastlog
    -r--------  1 root root 1254130450140 Jul  4 17:26 /var/log/lastlog
    # ls -lh /var/log/lastlog
    -r--------  1 root root 1.2T Jul  4 17:26 /var/log/lastlog

Why does the file appear to be 1.2 TB? This is because space is "allocated" ahead of time for all possible user IDs, which is about 232 users multiplied by 256 bytes for each login record, which is about 1.2 TB -- more or less. This is, thankfully, an illusion. The lastlog file is created as a sparse file, so only the chunks of the file that are used actually take up physical storage space. So all the space really isn't allocated. The file size just shows you how big the would would be if you tried to copy it or if you actually had 232 users.

To see how big the file really is use the `du` command:

    # du -h /var/log/lastlog
    76K     /var/log/lastlog
    76K seems like a much more reasonable size. Now try it with the '--apparent-size' option:

    # du -h --apparent-size lastlog
    1.2T    lastlog
    You can also us `ls` to detect sparse files with the '-s' option. Notice the '76' in the first column. This shows the actual physical number of blocks the file takes up -- only 76 blocks which is much less than 1254130450140 bytes:

    # ls -ls lastlog
    76 -r--------  1 root root 1254130450140 Sep 29 18:33 lastlog

