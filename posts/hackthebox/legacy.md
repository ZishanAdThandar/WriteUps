# Legacy - Machine

IP 10.10.10.4
Easy Windows

## Contents
- [Tools](#tools)
- [Enumeration](#enumeration)
- [Exploitation](#exploitation)

## Tools

- NMap
- Metasploit
   
## Enumeration

- Atfirst asusual running nmap with command `nmap -A 10.10.10.4` and it shows SMB is open. Server is running Windows.

## Exploitation

- After searching about SMB exploit got CVE-2008-4250. Then Searching more about CVE-2008-4250 shows a metasploit module `exploit/windows/smb/ms08_067_netapi`.
- Using that exploit in metasploit gives the "Administrator" shell on the machine. Now, we can get user and root flag. 

Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)
