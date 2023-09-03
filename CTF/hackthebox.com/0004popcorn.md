# Popcorn 10.10.10.6 Write Up

## Contents
- [Tools](#tools)
- [Enumeration](#enumeration)
- [Exploitation](#exploitation)
- [Priviledge Escalation](#priviledge-escalation)

Retired Medium Machine by ch4p

## Tools
1. Nmap
2. gobuster
3. Burp Suite Community Edition
4. SearchExploit (OFFSEC exploitDB)

## Enumeration

1. Nmap with `nmap -A 10.10.10.6` gives two open ports, port 22 for ssh and 80 for http.
2. On port 80 running gobuster with `gobuster dir -u http://10.10.10.6 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 40` gives some links as output. Output shows some links, `/test`, `/index`, `/torrent` etc.
3. `/test` page shows `phpinfo` allowed file upload.
4. On `/torrent` page we can see sign up and log in option. After loggin in we can use the upload option. during uploading torrent file, we can modify filename and filecontent to php shell.
<img src="./img/4b.png?raw=true" width="80%" alt="Burp"></li>


## Exploitation


## Priviledge Escalation



Author: Zishan Ahamed Thandar
