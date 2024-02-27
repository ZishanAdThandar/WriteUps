# Blue

- [Tools](#tools)
- [Recon](#recon)
- [Gain Access](#gain-access)

Room Link: https://tryhackme.com/room/blue

## Tools 

1. NMap https://nmap.org/download
2. 

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



Author: Zishan Ahamed Thandar
