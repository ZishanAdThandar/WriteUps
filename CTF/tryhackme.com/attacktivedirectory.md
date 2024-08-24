# Attacktive Directory

- [Tools](#tools)
- [Deploy The Machine](#deploy-the-machine)
- [Setup](#setup)
- [Welcome to Attacktive Directory](#Welcome-to-attacktive-directory)
- [Enumerating Users via Kerberos](#enumerating-users-via-kerberos)
- [Abusing Kerberos](#abusing-kerberos)
- [Back to the Basics](#back-to-the-basics)
- [Elevating Privileges within the Domain](#elevating-privileges-within-the-domain)
- [Flag Submission Panel](#flag-submission-panel)

Room Link: https://tryhackme.com/r/room/attacktivedirectory

## Tools


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

## Back to the Basics

## Elevating Privileges within the Domain

## Flag Submission Panel

Author: Zishan Ahamed Thandar
