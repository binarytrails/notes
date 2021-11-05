# powershell

## general

    Get-Host
    $PSVersionTable
    systeminfo

## process

    # start from cmd.exe
    start powershell

    # downgrade to v2
    powershell -v 2
    
    # bypass [e]xecution [p]olicy
    powershell.exe -ep bypass

    # disable security to install powershell script
    Set-ExecutionPolicy Bypass -Scope Process -Force;
    . .\script.ps1

    # become domain user
    runas /user:<domain>.local\<user> /netonly powershell.exe

    # run as administrator
    Start-Process .\myexe.exe -Verb runAs
    Start-Process powershell -Verb runAs

    # get process list
    Get-Process
    Get-Process | find-str \i "svchost"

    # kill a process
    taskkill /f /pid <pid>

    # get file content
    $text = Get-Content .\file.txt -Raw

    # output variable to file
    $text | Out-File c:\output.txt

## finding

    # find by case insensitive keyword
    findstr /? | findstr \i "color"

    # grep in files for a case insensitive string
    findstr /spin "mystring" /*

    # find a filename in drive
    Get-ChildItem C:\ -name -recurse something/

    # find a specific extension
    Get-ChildItem -Path .\ -Filter *.ico -Recurse -File

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

## dll

    # get dll exports
    dumpbin.exe /EXPORTS my.dll

    # find dumpbin (comes with visual studio)
    Get-ChildItem / -name -recurse dumpbin.exe

    # add dumpbin to env path
    $Env:path += ";C:\Program Files (x86)\Microsoft Visual Studio XX.Y\VC\bin\amd64"

    [Environment]::SetEnvironmentVariable("Path",[Environment]::GetEnvironmentVariable("Path", "User") + ";C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC\Tools\MSVC\<version>\bin\HostX64\x64","User")

## resources

    # monitor cpu and ram percentage usage
    $totalRam = (Get-CimInstance Win32_PhysicalMemory | Measure-Object -Property capacity -Sum).Sum
    while($true) {
        $date = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
        $cpuTime = (Get-Counter '\Processor(_Total)\% Processor Time').CounterSamples.CookedValue
        $availMem = (Get-Counter '\Memory\Available MBytes').CounterSamples.CookedValue
        $date + ' > CPU: ' + $cpuTime.ToString("#,0.000") + '%, Avail. Mem.: ' + $availMem.ToString("N0") + 'MB (' + (104857600 * $availMem / $totalRam).ToString("#,0.0") + '%)'
        Start-Sleep -s 2
    }

## obfuscation

    # command in base64
    $b=[Convert]::ToBase64String([Text.encoding]::UTF8.GetBytes("ls")
    [Text.encoding]::UTF8.GetString([Convert]::FromBase64String("$b")) |  Invoke-Expression

    # binary in base64
    $bin=cat bin.exe
    $b=[Convert]::ToBase64String([Text.encoding]::UTF8.GetBytes($bin))
    [Reflection.Assembly]::Load([Convert]::FromBase64String($b))
