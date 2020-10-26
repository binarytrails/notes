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

## common places

     c:\windows\system32\drivers\etc\hosts

## registry

    # Remove the 260 Character Path Limit
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem > LongPathsEnabled=1

## python

### venv

    C:\ENV\Scripts\activate.bat

## packet manager

### chocolatey

    # cmd.exe
    @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "[System.Net.ServicePointManager]::SecurityProtocol = 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

    # find where it installs
    findstr /spin "mypackage" C:\ProgramData\chocolatey\logs\chocolatey.log

