# VulnUniversity

- [Tools](#tools)
- [Deploy the machine](#deploy-the-machine)
- [Reconnaissance](#reconnaissance)
- [Locating directories using Gobuster](#locating-directories-using-gobuster)

Room Link: https://tryhackme.com/room/vulnversity

## Tools 

1. NMap https://nmap.org/download
2. Gobuster https://github.com/OJ/gobuster

## Deploy the machine 

1. Deploy The Machine by clicking Start The Machine
2. Download ovpn file and connect to the network using command `sudo openvpn --config username.ovpn`.

## Reconnaissance

1. Scan ports of the machine with given command `nmap -sV 10.10.65.81`
Output of the command:
```bash
Starting Nmap 7.80 ( https://nmap.org ) at 2024-02-26 09:55 IST
Nmap scan report for 10.10.135.130
Host is up (0.20s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.77 seconds
```
3. Question `how many ports are open?` Answer `6`
4. Question `What version of the squid proxy is running on the machine?` Answer `3.5.12`
5. Question `How many ports will Nmap scan if the flag -p-400 was used?` Answer `400`
6. Question `What is the most likely operating system this machine is running?` Answer `Ubuntu`
7. Question `What port is the web server running on?` Answer `3333`
8. Question `What is the flag for enabling verbose mode using Nmap?` Answer `-v`

## Locating directories using Gobuster 
   
1. 

Author: Zishan Ahamed Thandar




