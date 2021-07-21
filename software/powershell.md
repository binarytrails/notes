# powershell

## process

    # start from cmd.exe
    start powershell

    # run as administrator
    Start-Process .\myexe.exe -Verb runAs
    Start-Process powershell -Verb runAs

## env

    # gets the windows logical drives on the computer, drives mapped to network shares
    Get-PSDrive

    # get env vars
    $env:<path>

    Get-ChildItem env:
    Get-Childitem $Env:Temp -Recurse
    Get-ChildItem -Path Env:\SystemDrive                        # tab to complete
    Get-Childitem -Path Env:* | Sort-Object Name

    Get-Variable |%{ "Name : {0}`r`nValue: {1}`r`n" -f $_.Name,$_.Value }

    [System.Environment]::GetEnvironmentVariables()
    [System.Environment]::GetEnvironmentVariable('username')

    # set session env vars
    $TOTO="toto"
    $TOTO

    # set system env vars
    [System.Environment]::SetEnvironmentVariable('TOTO', 'toto')
    [System.Environment]::GetEnvironmentVariable('TOTO')

    # conditionals on env vars
    if([System.Environment]::GetEnvironmentVariable('username') -eq 'Administrator')
    {
       echo 'got admin!'
    }
    else
    {
       echo 'not admin...'
    }

## files

    mkdir /foo
    New-Item -Path C:\foo -Type Directory   # same as above
    New-Item -Path C:\foofile -Type File

    # show tree with folders and files
    tree /F

    # tail
    cat file.txt -Tail 10

    # grep -rni
    findstr /spin "mystring" /*

    # find / -name *toto*
    where /r c:\ my.exe
    Get-ChildItem c:\ -name -recurse something/

    # extract archive
    Expand-Archive file.zip -Destination /myfile

    # rm -rf
    Remove-Item .\folder -Recurse -Force

## networking

    # download
    Invoke-WebRequest <url> -Outfile file

## coding

    # cmake build instead of make
    cmake --build . --config Release

    # automate things
    echo "ls" > run-ls.ps1

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
