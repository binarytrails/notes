# general

## system time

    timedatectl status
    systemctl start systemd-timesyncd.service --now

    # find / set timezone
    timedatectl list-timezones |  egrep  -o “America/N.*”
    timedatectl set-timezone “Asia/Kolkata”
    timedatectl set-timezone Canada/Eastern # or

    # manually specify date / time
    timedatectl set-time "2020-01-02 07:51:00"

## hardware time

    hwclock --show

    # set to coordinated universal time (UTC)
    timedatectl set-local-rtc 0

    # set to local timezone
    timedatectl set-local-rtc 1

## remote time

    # synchronization with remote NTP server
    timedatectl set-ntp true

## date

    date +%Y-%m-%d

# issues

## E: Release file for ... is not valid yet

    timedatectl status
    systemctl start systemd-timesyncd.service --now
    sudo timedatectl set-timezone Canada/Eastern
    timedatectl status
    sudo timedatectl set-ntp on
