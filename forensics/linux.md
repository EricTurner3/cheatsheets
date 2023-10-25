# Linux Forensics

Notes taken for the EC-Council Computer Hacking Forensic Investigator (CHFI) v10.

Linux comes in many different flavors known as distros. Every distro has a different set of default installed applications, so some of these commands may require package installation. Native alternatives are provided when known.

1. Collecting Volatile Data
1. Collecting Non-Volatile Data
1. File Integrity
1. File and File System Analysis
1. Memory Analysis

## Collecting Volatile Data
Volatile Data is data that will be destroyed/lost if a computer loses power, is shut down or logged off. It is the most critical data to capture first in forensics.
* Host Information
    * `hostname` - computer name
* User Information
    * `w` - currently logged in users, timestamp, host ip, idle, current process
* Time Information
    * `date && cat /etc/timezone` - date and timezone
        * `date +%s` - date in unix epoch
    * `uptime` - time since last restart, also includes current time, # of logged-in users, system load averages.etc
* Network Information
    * `ifconfig` - show network adapters and if in promiscuous mode
    * `ip`
        * `ip addr show` - show all network adapters, status and IP
        * `ip r` - show routing table
    * `netstat`
        * `netstat -i` - show network interfaces
        * `netstat -rn` - show routing table
        * `netstat -tulpn` - shows open TCP/UDP/listening ports with associated PIDs
* Port Information
    * `nmap` - port scanner
        * May not be installed by default depending on distro
        * `nmap -sT localhost` - show open TCP ports
        * `nmap -sU localhost` - show open UDP ports
    * `lsof -i -P -n | grep LISTEN` - alternate command for listening ports
* Open Files
    * `lsof` - all open files with the name of process
        * `lsof -u <user>` - all open files for current logged in user
* Mounted File Systems
    * `mount` - shows list of mounts
    * `df` - disk space used for filesystems
* Loaded Kernel Modules
    * `lsmod` - lists all currently loaded kernel modules
    * `modinfo <kernel_module>` - info about a specific kernel module
* User Events and ELF Files
    * `id <username>` - get user ID for specified username
    * `ausearch -ul <user_id> --interpret` - track all user events for a specified user ID
        * may not be installed by default depending on distro, part of `auditd`
    * `readelf` - analyze headers and sections of linux executables
        * may not be installed by default depending on distro, part of `binutils`
* Running Processes
    * `ps` - view running proceses
    * `pstree` - view process tree
* Swap Areas and Disk Partition Info
    * `cat /proc/partitions` - list partitions
    * `cat /proc/swaps` - check swap size and swap areas
* Kernel Messages
    * `dmesg` - display kernel ring buffers or drivers loaded into kernel


## Collecting Non-Volatile Data
This data is not time-sensitive and should remain unchanged after a logoff, shutdown or power loss.
* System Information
    * `cat /proc/cpuinfo` - view cpu info
    * `cat /proc/self/mounts` - view mount points and mounted external devices
    * `cat /etc/lsb-release` - distro information
* Kernel Information - any one of the below commands
    * `uname -r`
    * `cat /proc/version`
    * `hostnamectl | grep Kernel`
* User Account & Login Information
    * `cat /etc/passwd` - view user file with asscoate uid, group id, home dir and default shell
        * `cut -d: -f1 /etc/passwd` - retrieve only the usernames from the file
        * `/etc/shadow` contains the encrypted passwords for the users, usually requires root permissions
    * `last -f /var/log/wtmp` - login history
    * `cat /var/log/auth.log` - log of authorization and login events
        * `grep sudo /var/log/auth.log` - view only sudo commands from auth log
* Log Files
    * Log files are found under `/var/log`, notable ones:
        * `cat /var/log/syslog` - syslog data
        * `cat /var/log/kern.log` - kernel logs
        * `cat /var/log/auth.log` - auth logs 
        * `cat /var/log/faillog` - failed login attempts
        * `cat /var/log/lpr.log` - printer logs
        * `cat /var/log/mail.*` - mail server logs
        * `cat /var/log/dpkg.log` - package installation or removal
        * `cat /var/log/daemon.log` - running services
        * `cat /var/log/debug` - debug logs
* User History File
    * `history` to view commands previously executed by currently logged in user
    * `cat /home/<user>/.bash_history` - view bash history for another user (reqs root perms)
* Hidden Files and Directories
    `ls -la`
* Collecting Suspicious Information
    * `rkhunter --check --rwo` - find suspicious files under /dev
        * will require install
    * `chkrootkit` - find for signs of anomalies
        * will require install

## File Integrity
* After the collection of all evidence, `md5sum` or `sha1sum` should be ran on gathered image files to compare against the hash after analysis has occured. This ensures the collected evidence was not modified during analysis.

## File and File System Analysis
* File Analysis
    * `xxd <file> | head` - view first few lines of file in hex, useful for checking magic bytes in beginning
    * `file` - identify filetype when file has no extension
    * `strings | grep` - strings and grep are useful for searching for text strings in any file
    * `find / -writable -type f 2>/dev/null` - search for all writable files
        * pipe into `grep` to find files in a specific directory
* File System Analysis using The Sleuth Kit
    * `fsstat` - provides info about a file system
    * `fls` - list files and directories in an image file
    * `istat` - displays metadata about files

## Memory Analysis
* [Volatility](https://www.volatilityfoundation.org/) - universal extensive python framework for memory analysis
* `strings`
    * strings + grep/egrep can be used to discern information from a memory file
    * files & dirs: `strings memfile.mem | grep -i "^[a-z]:\\\\" | sort | uniq`
    * URLs: `strings memfile.mem | egrep "^https?://" | sort | uniq`
    * IPs: `strings memfile.mem | egrep -oE "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}"`
        * Only checks for ###.###.###.### and not a true valid IP.
* Hex Editors, such as [WinHex](https://www.x-ways.net/winhex/)