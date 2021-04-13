# Powershell

    mkdir /foo
    New-Item -Path C:\foo -Type Directory   # same as above
    New-Item -Path C:\foofile -Type File

    # get env vars
    $env:path

    Get-ChildItem env:
    Get-ChildItem -Path Env:\SystemDrive                        # tab to complete

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
