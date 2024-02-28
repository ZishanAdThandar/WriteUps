# The Impossible Challenge

- [Tools](#tools)
- [Deploy the vulnerable machine](#Deploy-the-vulnerable-machine)
- [Enumerating Samba for shares](#enumerating-samba-for-shares)
- [Gain initial access with ProFtpd](#gain-initial-access-with-progtpd)
- [Privilege Escalation with Path Variable Manipulation](#privilege-escalation-with-path-variable-manipulation)
  
Room Link: https://tryhackme.com/room/kenobi

## Tools 

1. NMap
2. 

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

## Gain initial access with ProFtpd
## Privilege Escalation with Path Variable Manipulation 

Author: Zishan Ahamed Thandar
