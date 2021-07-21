# windows

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

## finding windows activation key

    # cmd.exe (sometimes absent here)
    slmgr/dli

    # powershell (always present if in BIOS)
    (Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey

## disable updates / reboots

### windows 10

1. Click Start and type Task Scheduler

    1. Navigate to Task Scheduler Library >> Microsoft >> Windows >> UpdateOchestrator

    2. To disable automatic reboots right-click on Reboot and select disable.

2. Use Windows Update GPO.

    1. Open your start menu and type Group, then click Edit group policy

    2. Expand Computer Configuration \ Administrative Templates \ Windows Components \ Windows Update

    3. Double click Configure Automatic Updates and enable the policy, and configure it as needed.

    4. Also other group policy in Windows Update could be helpful. For example: No auto-restart with logged on users for scheduled automatic update installations

3. Use Registry for WindwosUpdate

    1. Press Win + R and type regedit then hit Enter

    2. Navigate to HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\WindowsUpdate\AU (you may need to create the keys manually if they don't exist)

    3. Create a new DWORD value called AUOptions and enter a value of either 2 or 3.

        2 = Notify before download

        3 = Automatically download and notify of installation

4. User Registry to Disabled Windows Software Protection Service

    1. HKLM/SYSTEM/CurrentControlSet/Services/sppsvc

    2. Set Start value to 4

In order for everything to work Reboot.

## Event ID

    # Event ID 1074 - Random and unsolicited system shutdown
    # Filter for a shutdown event Event:
    eventvwr.msc > Windows > System Log > Filter with Event ID '1074'

## disable Windows Defender (win10)

temporary in settings:

    Windows Security > Virus & threat protection > Virus & threat protection settings
    Turn off Real-time protection
    Turn off the Tamper Protection toggle switch

permanetly in policies:

    gpedit.msc > Local Group Policy Editor > Computer Configuration > Administrative Templates > Windows Components > Microsoft Defender Antivirus > Double-click the Turn off Microsoft Defender Antivirus policy > Select the Enabled option to disable Microsoft Defender Antivirus
