<h1>Legacy 10.10.10.3 Write Up</h1>

<h2>Contents</h2>

- [Tools](#tools)
- [Enumeration](#enumeration)
- [Exploitation](#exploitation)

Retired Easy Machine by ch4p

## Tools

1. NMap
2. Metasploit
   
## Enumeration

1. Atfirst asusual running nmap with command `nmap -A 10.10.10.4` and it shows SMB is open. Server is running Windows.

## Exploitation

<img src="./img/2a.png?raw=true" width="80%" alt="Metasploit">

1. After searching about SMB exploit got CVE-2008-4250. Then Searching more about CVE-2008-4250 shows a metasploit module `exploit/windows/smb/ms08_067_netapi`.
2. Using that exploit in metasploit gives the "Administrator" shell on the machine. Now, we can get user and root flag.  

Author: [Zishan Ahamed Thandar](https://github.com/ZishanAdThandar/WriteUps/tree/main?tab=readme-ov-file#about-me)
