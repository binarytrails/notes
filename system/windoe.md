# windoe

## powershell

    # start from cmd.exe
    start powershell

    # tail
    cat file.txt -Tail 10

    # grep -rni
    findstr /spin "mystring" /*

    # find / -name *toto*
    where /r c:\ my.exe
    Get-ChildItem c:\ -name -recurse something/

    # download
    Invoke-WebRequest <url> -Outfile file

    # extract archive
    Expand-Archive file.zip -Destination /myfile

    # cmake build instead of make
    cmake --build . --config Release

    # automate things
    echo "ls" > run-ls.ps1

    # rm -rf
    Remove-Item .\folder -Recurse -Force

### event logs

    Get-EventLog *
    Get-EventLog -LogName System -Newest 20
    Get-EventLog -LogName System -EntryType Error
    Get-EventLog -LogName System -Message *something in the message*
    Get-EventLog -LogName System -After '11/10/2020 07:00:00'

    $MyEvent = Get-EventLog -LogName System -Newest 1
    $MyEvent | Select-Object -Property *

    Get-EventLog -LogName System -UserName NT* | Group-Object -Property UserName -NoElement | Select-Object -Property Count, Name

    Get-WinEvent -ListLog * | Format-List -Property LogName
    (Get-WinEvent -ListLog Security).ProviderNames

## common places

     c:\windows\system32\drivers\etc\hosts

## registry

    # Remove the 260 Character Path Limit
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem > LongPathsEnabled=1

    # Disable auto reboot
    HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU > NoAutoRebootWithLoggedOnUsers=1

## python

### venv

    C:\ENV\Scripts\activate.bat

## packet manager

### chocolatey

    # cmd.exe
    @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "[System.Net.ServicePointManager]::SecurityProtocol = 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

    # find where it installs
    findstr /spin "mypackage" C:\ProgramData\chocolatey\logs\chocolatey.log

## partitioning

    mkfs.msdos -F 32 /dev/sdx

## mount from arch linux

    pacman -S ntfs-3g
    mount /dev/sdX /mnt
