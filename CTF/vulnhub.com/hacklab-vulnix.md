# HackLAB: Vulnix

- [Tools](#tools)
- [Getting Access](#getting-ccess)
- [Priviledge Escalation](#priviledge-escalation)

Machine: https://www.vulnhub.com/entry/hacklab-vulnix%2C48/

Now we can Download the 7z file and solve the machine by hosting it inside a virtual box.
## Tools
1. [NMap](https://nmap.org/)
2. [hydra](https://github.com/vanhauser-thc/thc-hydra)
3. ssh

## Getting Access
1. I used the Bridged Adapter setting so my ip is 192.168.0.8.
2. At first scanned with nmap. Nmap Shows some open ports 22 ssh, 25 smtp, 79 finger, 110 POP3, 111 rpcbind etc. 
3. If we run finger user enumeration script of pentestermonkey with command `perl finger-user-enum.pl -U /opt/metasploit-framework/embedded/framework/data/wordlists/unix_users.txt -t IP_ADRESS` we can get many usernames including user.
4. If we bruteforce port 22 for ssh with hydra, we will get the password for user user is `letmein`. We can use command, `hydra -l user -P /opt/wordlist/rockyou.txt 192.168.0.8 ssh -t 4` to bruteforce ssh with hydra.
5. If we login to ssh and check id we can get an user inside `/etc/passwd` as vulnix 2008.
6. After creating a user as 2008 vulnix, we can `mount /home/vulnix` using nfs. 
7. Generate ssh key with ssh-keygen
8. Now upload ssh key to the vulnix machine 
9. Now we can `ssh` to the machine, with `ssh vulnix@192.168.0.8`.
    
## Priviledge Escalation  
1. `sudo -l` shows `/etc/exports` is editable. So, added `/root  *(rw,sync,no_root_squash)` to root.
2. Now rebooting the VM will add root to nfs, and we can mount root directory. 
3. So we got the flag inside `trophy.txt` is `cc614640424f5bd60ce5d5264899c3be`.

Author: Zishan Ahamed Thandar



