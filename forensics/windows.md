# Windows Forensics

Notes taken for the EC-Council Computer Hacking Forensic Investigator (CHFI) v10

1. Collecting Volatile Data
1. Collecting Non-Volatile Data
1. File Integrity
1. Memory Analysis
1. Registry Analysis
1. Cache, Cookie, and History Analysis
1. File Analysis
1. Metadata Investigation
    1. Shellbags
    1. LNK Files
    1. Jump Lists
1. Event Logs Analysis


## Collecting Volatile Data
Volatile Data is data that will be destroyed/lost if a computer loses power, is shut down or logged off. It is the most critical data to capture first in forensics.
* System Time
    * useful if machine is using an incorrect time in order to properly re-create the timeline / map to the correct time
    * `date /t & time/t`
* Logged-In Users / Sessions
    * `net sessions`
    * [PsLoggedOn](https://learn.microsoft.com/en-us/sysinternals/downloads/psloggedon)
    * [LogonSessions](https://learn.microsoft.com/en-us/sysinternals/downloads/logonsessions)
* Shared Folders and Files
    * `net file` - view file/folders shared via network
    * [NirSoft's NetworkOpenedFiles](https://www.nirsoft.net/utils/network_opened_files.html) - view files currently being opened both locally & via network
* Network Information
    * `nbtstat` 
        * `nbtstat -c` - show contents of name cache
    * `netstat` - view connections and routes
    * `ipconfig` - view current network adapter information
    * [PromiscDetect](https://vidstromlabs.com/freetools/promiscdetect/) - 3rd party tool that checks if any network adapters are running in promiscuous mode
    * [PromQry](https://www.microsoft.com/en-us/download/details.aspx?id=1851) - same as above, but from Microsoft
* Process Information
    * `tasklist`
    * `pslist`
    * `netstat -ano` - switches that will map running process to ports
    * [ListDLLs](https://learn.microsoft.com/en-us/sysinternals/downloads/listdlls) - list all DLLs loaded for a process, including digital signatures
    * [Handle](https://learn.microsoft.com/en-us/sysinternals/downloads/handle) - displays info about ports, registry keys, threads.etc
* Process Memory
    * [Process Explorer](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer)
    * [ProcDump](https://learn.microsoft.com/en-us/sysinternals/downloads/procdump)
    * [Process Dump](https://github.com/glmcdona/Process-Dump)
* Memory / RAM Collection
    * [WinPMem](https://github.com/Velocidex/WinPmem) - windows only, free
    * [Linpmem](https://github.com/Velocidex/Linpmem) - *nix only, free
    * [Avml](https://github.com/microsoft/avml) - *nix only, free
    * [FTK Imager](https://www.exterro.com/ftk-imager) - universal, some features require purchase
        * can also pull non-volatile information such as page file
* Printer Spool Files
    * Default in C:\Windows\System32\spool\PRINTERS
    * Files can be viewed in a hex editor such as WinHex
* Clipboard Contents
    * Win10+: Keyboard Shortcut `WIN + V` will open your clipboard history
    * [Free Clipboard Viewer](https://freeclipboardviewer.com/) pulls clipboard information from memory
* Service / Driver Information
    * `wmic service list brief`
* Command History
    * Command Prompt: `doskey /history`
    * PowerShell: `Get-History`
* Windows Registry
    * Hives
        * HKEY_CLASSES_ROOT - application config info
        * HKEY_CURRENT_USER - currently logged-on user info
        * HKEY_CURRENT_CONFIG - hardware profile
    * Entire registry can be exported via `regedit`, right clicking on Computer and clicking Export
    * [FTK Imager](https://www.exterro.com/ftk-imager) can also be used


## Collecting Non-Volatile Data
This data is not time-sensitive and should remain unchanged after a logoff, shutdown or power loss.
* Extensible Storage Engine (ESE) Files
    * .edb files, can store information about Microsoft services such as mail, search indexing, update information, IE favorites and history
    * Can be viewed / extracted with [ESEDatabaseView](https://www.nirsoft.net/utils/ese_database_view.html)
* External Devices
    * Known connected USB Devices can be viewed via RegEdit under `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR`
    * [Windows Device Console](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/devcon) - document devices attached to system
    * [DriveLetterView](https://www.nirsoft.net/utils/drive_letter_view.html) - lists all drives on system even if currently disconnected
* Slack Space
    * Whitepaper, [Analysis of Hidden Data in the NTFS File System](https://www.forensicfocus.com/articles/analysis-of-hidden-data-in-the-ntfs-file-system/)
        * `chkdsk`
        * `mmls`
        * `fsstat`
* Prefetch Files - C:\Windows\Prefetch
* Browser Cache Data
    * Usually found under AppData\Local
* Windows Thumbnail Cache
    * thumbnailcache_***.db, contains original filename, timestamps, EXIF data even of deleted files
    * [Thumbcache Viewer](https://thumbcacheviewer.github.io/)
    * [Thumbs Viewer](https://thumbsviewer.github.io/)
* Windows Crash Dump Data
    * [DumpChk](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/dumpchk)
* Hibernate Files
    * C:\hiberfil.sys - memory dump when computer goes into hibernation
* Windows Registry
    * HKEY_LOCAL_MACHINE - system configuration information
        * contains the SAM, which contains the users and passwords for the devices
    * HKEY_USERS - actively loaded user profiles

## File Integrity
* After the collection of all evidence, `Get-FileHash` should be ran on gathered image files to compare against the hash after analysis has occured. This ensures the collected evidence was not modified during analysis.

## Memory Analysis
After collecting the data, these tools are useful in then reviewing the data
* [Redline](https://fireeye.market/apps/211364) - Windows GUI for memory and file analysis
* [Volatility](https://www.volatilityfoundation.org/) - universal extensive python framework for memory analysis
* `strings`
    * strings + grep/egrep can be used to discern information from a memory file
    * files & dirs: `strings memfile.mem | grep -i "^[a-z]:\\\\" | sort | uniq`
    * URLs: `strings memfile.mem | egrep "^https?://" | sort | uniq`
    * IPs: `strings memfile.mem | egrep -oE "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}"`
        * Only checks for ###.###.###.### and not a true valid IP.
* Hex Editors, such as [WinHex](https://www.x-ways.net/winhex/)

## Registry Analysis
* Registry contains a LastWrite time that can be used for timeframe reference
* `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet` contains system information
    * `.\Control\ComputerName\ActiveComputerName` - computer name
    * `.\Control\TimeZoneInformation` - system's timezone
    * `.\Services\LanmanServer\Shares` - user created shares
* `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion` - current OS version
* `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles\{GUID}` - Wireless SSID information
* `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Windows` - last shutdown time
* User Login / AutoRun - not executed in Safe Mode
    * `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
    * `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
    * `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
    * `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
* User Security IDs (SID)
    * `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Current Version\ProfileList`
* `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR` - USB Removable Storage Devices
* `HKEY_LOCAL_MACHINE\SYSTEM\Mounted Devices` - mounted devices (shows drive letters)
* `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count` - UserAssist Keys, values are ROT-13 encoded
* `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` - Most Recently Used file list
* `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RunMRU` - Most Recently Used executable list (exes started from the run.exe window)
* `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Internet Explorer\TypedURLs` - URLs typed into address bar
* `\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Map Network Drive MRU` - network mapped locations
* `\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2` - volumes mounted via `net use`

## Cache, Cookie and History Analysis
* Browser Cache Data
    * NirSoft has a collection of Cache, History and Cookie extractors for IE, Firefox, Opera, Safari, Chrome and more [here](https://www.nirsoft.net/web_browser_tools.html).

## File Analysis
* System Restore Points
    * rp.log
    * change.log.x
* Prefetch files - information about installation, and running of application
    * [WinPrefetchView](http://www.nirsoft.net/utils/win_prefetch_view.html)
* Image Files - can contain valuable EXIF information that includes timestamps, camera/lens information, location information
    * [ExifTool](https://exiftool.org/)
    * [Exiv2](https://exiv2.org/) - allows overwritting the metadata

## Metadata Analysis
* Timestamps, user owner information can usually be found on all files
* Documents, such as word or PDF, can have Organization Info, Author Info, Template Info
### Shellbags
Information on folders that have been visited by the user in Windows Explorer. Shellbags are generated when a folder is opened and closed at least once, and even shows directories that have been removed or directories on drives that are no longer mounted.
* Can be found in the registry
    * `HKEY_CURRENT_USER\Software\Microsoft\Windows\Shell\BagMRU`
    * `HKEY_CURRENT_USER\Software\Microsoft\Windows\Shell\Bags`
    * `HKEY_CURRENT_USER\Software\Classes\Local Settings\Software\Microsoft\Windows\Shell\BagMRU`
    * `HKEY_CURRENT_USER\Software\Classes\Local Settings\Software\Microsoft\Windows\Shell\Bags`
* [ShellBags Explorer](https://www.sans.org/tools/shellbags-explorer/) - GUI tool for browsing shellbags data

### LNK Files
Shortcut files used by windows, can provide valuable info on user activies.
* Stored in `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent`
* [LECmd](https://github.com/EricZimmerman/LECmd) - command line utility for browsing LNK files

### Jump Lists
Feature in Win7+, shows recently accessed apps, files and actions when right clicking an item on the taskbar
* `C\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations` - created when any user **accesses** a app pinned to the taskbar
* `C\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations` - created when a user **pins** a file or app to the taskbar
* [JumpListExt](https://sourceforge.net/projects/jumplistext) - GUI Jump List parser

## Event Log Analysis
* Win10 stores event logs in .evtx, based off of XML
* `wevtutil` can retrieve event logs that are hidden in the GUI Event Viewer
* [Most Common Windows Event IDs to Hunt](https://www.socinvestigation.com/most-common-windows-event-ids-to-hunt-mind-map) - article on top event IDs to look out for, from a SOC perspective
* [Event Log Explorer](https://www.eventlogxp.com/) - 3rd party tool for event log analysis

