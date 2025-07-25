# Windows

<!-- INDEX_START -->

- [Remote Desktop](#remote-desktop)
- [Screenshots](#screenshots)
- [Start At Login](#start-at-login)
- [MMCs](#mmcs)
- [WinDirStat - Disk Space Analysis](#windirstat---disk-space-analysis)
- [Commands](#commands)
  - [Networking](#networking)
  - [List Volumes](#list-volumes)
  - [Get Free Disk Space on All Volumes](#get-free-disk-space-on-all-volumes)
  - [Disk Check Analysis](#disk-check-analysis)
  - [Find the location of a binary in the %PATH%](#find-the-location-of-a-binary-in-the-path)
  - [List Disk Space](#list-disk-space)
  - [File Permissions](#file-permissions)
  - [Base64 encode a file](#base64-encode-a-file)
  - [Copy file to clipboard](#copy-file-to-clipboard)
  - [Paste clipboard to file](#paste-clipboard-to-file)
  - [Open file from the command line using default application](#open-file-from-the-command-line-using-default-application)
- [WSL - Windows Subsystem for Linux](#wsl---windows-subsystem-for-linux)
- [GitHub Repos](#github-repos)
- [Meme](#meme)
  - [Headquarters](#headquarters)
  - [Wizard Install Software](#wizard-install-software)
  - [Best Keyboard for Programming](#best-keyboard-for-programming)
  - [Windows Problems vs Linux Problems](#windows-problems-vs-linux-problems)
  - [Linux Users Switching Back After 10 Minutes of Using Windows](#linux-users-switching-back-after-10-minutes-of-using-windows)

<!-- INDEX_END -->

## Remote Desktop

You can find your corporate workspace by downloading
[Remote Desktop app](https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466)
from the Apple Store and then Add Workspace and entering this URL:

<https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>

You'll be prompted for your Microsoft login, enter your company corporate email and password, and your RDP session will
be seen as an icon to click through to.

This icon will remain under Workspaces, not PCs when you are starting RDP again to connect.

This conveniently also reconnects if you leave the app running and move locations / wifi as a digital nomad or even just
going home.

On Mac use the `Cmd` key as the Windows key.

<!--

Doesn't work, likely due to AD Group Policies

### Disable the Screensaver

You may want to disable the screensaver on your Windows Virtual Desktop since it is protected by the screensaver on
your local machine.

If you are an Administrator on your Windows Virtual Desktop but the Domain Admins have applied a policy
to stop you launching the `Change screen saver` dialog:

![Windows display control panel disabled](images/windows_display_control_panel_disabled.png)

Then you can use the following command to work around it in `cmd.exe`:

```commandline
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v ScreenSaveActive /t REG_SZ /d 0 /f
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v ScreenSaverIsSecure /t REG_SZ /d 0 /f
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v SCRNSAVE.EXE /t REG_SZ /d "" /f
```

Verify it is set to `0`:

```commandline
reg query "HKEY_CURRENT_USER\Control Panel\Desktop" /v ScreenSaveActive
reg query "HKEY_CURRENT_USER\Control Panel\Desktop" /v ScreenSaverIsSecure
reg query "HKEY_CURRENT_USER\Control Panel\Desktop" /v SCRNSAVE.EXE
```

Reversing this is just setting it to `1` instead.

-->

## Screenshots

Screenshots can be tricky when using WVD above from a Mac
as the Print Screen key isn't available on Mac and shortcuts like `Windows`-`Shift`-`S`
don't work.

Pull up the Snipping Tool from the task search and use that to take a screenshot of a given area.

On Mac hit the `Cmd` key which is equivalent of the Windows key inside the WVD session to bring up the task bar search
and then type`snip` and enter.

Once the Snipping Tool is up, click New and then drag a selection window and save it as a screenshot file to share.

## Start At Login

To have any program start at login, such as Teams or Outlook to save you clicks
(especially if you're starting new WVD sessions every day or every 4 hours):

`Start` -> `Run`:

This opens the Startup folder for your user account:

```commandline
shell:startup
```

Then just drag a shortcut of the app into that folder:

eg. `Start` and drag the icon of the app over that Startup folder to create the shortcut.

Test by logging out and back in.

## MMCs

Microsoft Management Consoles are UI utilities to administer the system.

You can launch them from `Start` -> `Run` menu and typing their name which ends in `.msc`:

| Command          | Name                                 | Description                                                                  |
|------------------|--------------------------------------|------------------------------------------------------------------------------|
| `compmgmt.msc`   | Computer Management                  | Comprehensive console with tools like Disk Management, Task Scheduler ...    |
| `diskmgmt.msc`   | Disk Management                      | Manages disk drives and partitions                                           |
| `taskschd.msc`   | Task Scheduler                       | Automates task execution based on specific conditions                        |
| `services.msc`   | Services                             | Displays and manages all services installed on the system                    |
| `devmgmt.msc`    | Device Manager                       | Interface to view and manage hardware devices installed on the computer      |
| `dsa.msc`        | Active Directory Users and Computers | Manages users, computers, and objects in a domain                            |
| `lusrmgr.msc`    | Local Users and Groups               | Manages local user accounts and groups                                       |
| `gpedit.msc`     | Group Policy Management              | Centralized management environment for configuring Group Policy settings     |
| `perfmon.msc`    | Performance Monitor                  | Monitors system performance, displaying real-time data about system usage    |
| `eventvwr.msc`   | Event Viewer                         | Allows viewing and managing system, application, and security event logs     |

## WinDirStat - Disk Space Analysis

Use the excellent [WinDirStat](https://windirstat.net/) tool to visually see what is occupying your disk space.

[Disk Inventory X](https://www.derlien.com/) for Mac and [KDirStat](https://kdirstat.sourceforge.net/) for Linux are
inspired by this.

## Commands

Show the current time:

```commandline
time /t
```

(without the `/t` switch it prompts to set a new time)

### Networking

See [Networking](networking.md) doc.

### List Volumes

```commandline
fsutil volume list | findstr :
```

### Get Free Disk Space on All Volumes

```commandline
powershell "Get-PSDrive -PSProvider FileSystem | Select-Object Name, @{Name='FreeSpace(GB)';Expression={($_.Free/1GB).ToString('F2')}}, @{Name='UsedSpace(GB)';Expression={((($_.Used)/1GB).ToString('F2'))}}, @{Name='TotalSize(GB)';Expression={($_.Used+$_.Free/1GB).ToString('F2')}}"
```

### Disk Check Analysis

```commandline
chkdsk c:
```

output:

```text
Windows has scanned the file system and found no problems.
No further action is required.

  51796991 KB total disk space.
  30478208 KB in 771533 files.
    438244 KB in 283686 indexes.
         0 KB in bad sectors.
   1194783 KB in use by the system.
     55232 KB occupied by the log file.
  19685756 KB available on disk.

      4096 bytes in each allocation unit.
  12949247 total allocation units on disk.
   4921439 allocation units available on disk.
```

### Find the location of a binary in the %PATH%

```commandline
where bash
```

output:

```commandline
C:\Program Files\Git\bin\bash.exe
```

### List Disk Space

```commandline
fsutil volume diskfree c:
```

### File Permissions

Set owner of file administrator:

```commandline
icacls "D:\test\test.txt" /setowner "administrator"
```

Recursively change `/app` directory and its contents be readable by all users

```commandline
icacls "D:\test\test.txt" /grant "users:(R)" /t
```

Grant full control permission to administrator:

```commandline
icacls "D:\test\test.txt" /grant "administrator:(F)"
```

Grant read and execute to all users:

```commandline
icacls "D:\test\test.txt" /grant "users:(RX)"
```

### Base64 encode a file

Use the native `certutil` command:

```commandline
certutil -encode "%file%" "$base64_file"
```

You'll need to strip off the leading `-----BEGIN CERTIFICATE-----` and trailing `-----END CERTIFICATE-----` in that file
before you use it.

### Copy file to clipboard

For text files only (like the above base64 file):

```commandline
set "file=C:\path\to\your\file.txt"
```

```commandline
type "%file%" | clip
```

or using PowerShell:

```powershell
# For text files
Get-Content "path\to\your\file.txt" -Raw | Set-Clipboard

# For binary files (converting to base64 first)
[Convert]::ToBase64String([System.IO.File]::ReadAllBytes("path\to\your\file.jpg")) | Set-Clipboard
```

### Paste clipboard to file

In cmd:

```commandline
powershell -command "Get-Clipboard" > "%file%"
```

or in Powershell:

```powershell
Get-Clipboard | Out-File -FilePath "%file%
```

### Open file from the command line using default application

```shell
start "%file%"
```

or if the filename has spaces in it:

```shell
start "" "%file%"
```

## WSL - Windows Subsystem for Linux

```commandline
wsl --list --oneline
```

shorter version:

```commandline
wsl -l -o
```

```commandline
wsl install
```

You will be prompted for a username and password with admin privileges.

You will need to reboot (you won't even be able to run `wsl -l -o` again until you do):

```commandline
shutdown /r
```

## GitHub Repos

- [:octocat: scipag/HardeningKitty](https://github.com/scipag/HardeningKitty)
- [:octocat: Raphire/Win11Debloat](https://github.com/Raphire/Win11Debloat) - PowerShell script to remove pre-installed
  apps, disable telemetry, customization, declutter and improve your Windows experience. Works on Windows 10 and 11

## Meme

### Headquarters

![Headquarters](images/headquarters_io_windows_linux.jpeg)

### Wizard Install Software

How many of you dweebs are still using Windows instead of Linux or Mac?

![Wizard Install Software](images/windows_wizard_install_software.jpeg)

### Best Keyboard for Programming

![Best Keyboard for Programming](images/best_keyboard_for_programming.jpeg)

### Windows Problems vs Linux Problems

Mac falls somewhere in between the two, depending on the problem...

![Windows Problems vs Linux Problems](images/windows_problems_reboot_linux_problems_be_root.jpeg)

### Linux Users Switching Back After 10 Minutes of Using Windows

![Linux Users Switching Back After 10 Minutes of Using Windows](images/linux_users_switching_back_after_10_mins_using_windows.jpeg)
