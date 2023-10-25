# MacOS Forensics

Notes taken for the EC-Council Computer Hacking Forensic Investigator (CHFI) v10.

1. Collecting Memory
1. Collecting Forensics Data
1. Memory Analysis

## Collecting Memory
* [FireEye Memoryze](https://fireeye.market/apps/211372) - freeware, can acquire and analyze memory images on live systems

## Collecting Forensics Data
Forensics Data on Mac OS X / macOS is mostly stored in various locations on the filesystem. They can be read out via Terminal using `cat`, or in your preferred Text Editor
* System Version - `cat /System/Library/CoreServices/SystemVersion.plist`
* Timestamps 
    * `stat` can be used for uptime or timestamps of files, apps, logs.etc
* User Account Information - `/Users/<username>/Library`
    * Keychains - `./KeyChains` and are 3DES encrypted by the master password
    * Browser information - `./Application Support/<Vendor>` with vendor as `Firefox` or `Google/Chrome`
    * Startup Items - `./StartupItems`
    * Application Prefences -`./Preferences`
    * Trash - `./Trash`
    * Social Media Accounts `./Accounts/Accounts3.sqlite`
* Kernel Extensions (Kexts)
    * Located in `/System/Library/Extensions` - malicious extensions can also be installed in this directory
* Log Files
    * Similar to linux, found under `/var/log/`
    * Can also be found under `/Users/<username>/Library/Logs`
* Spotlight
    * Spotlight is the search engine in MacOS
    * Spotlight search history is saved in `/.Spotlight-V100/Store-V2/<UUID>/.store.db` 
        * Since macOS Sierra (10.13), it can also be found under `/Users/<username>/Library/Metadata/CoreSpotlight/index.spotlightV3`
        * it can be parsed into a csv with [spotlight_parser](https://github.com/ydkhatri/spotlight_parser)
* File System Analysis
    * Prior to macOS Sierra (10.13), default file system was HFS or HFS+
        * disk drives or images can be analyzed with [HFSExplorer](https://catacombae.org/hfsexplorer/)
    * Since macOS Sierra (10.13), the default file system is APFS
        * [Biskus APFS Capture](https://biskus.com/) - no longer available

## Memory Analysis
* [Volatility](https://www.volatilityfoundation.org/) - universal extensive python framework for memory analysis
* `strings`
    * strings + grep/egrep can be used to discern information from a memory file
    * files & dirs: `strings memfile.mem | grep -i "^[a-z]:\\\\" | sort | uniq`
    * URLs: `strings memfile.mem | egrep "^https?://" | sort | uniq`
    * IPs: `strings memfile.mem | egrep -oE "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}"`
        * Only checks for ###.###.###.### and not a true valid IP.
* Hex Editors, such as [WinHex](https://www.x-ways.net/winhex/)