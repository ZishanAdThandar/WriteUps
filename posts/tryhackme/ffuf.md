# ffuf

- [Tools](#tools)
- [Introduction](#introduction)
- [Basics](#basics)
- [Finding Pages and Directories](#finding-pages-and-directories)
- [Using Filters](#using-filters)
- [Fuzzing Parameters](#fuzzing-parameters)
- [Finding Vhosts and Subdomains](#finding-vhosts-and-subdomains)
- [Proxifying FFUF Traffic](#proxifying-ffuf-traffic)
- [Reviewing the Options](#reviewing-the-options)
- [About the Author](#about-the-author)

Room Link: [https://tryhackme.com/r/room/ffuf](https://tryhackme.com/r/room/ffuf)

```bash

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

```

## Tools

- [HackiFy](https://github.com/ZishanAdThandar/HackiFy)
- [ffuf](https://github.com/ffuf/ffuf)
- [SecLists](https://github.com/danielmiessler/SecLists)

## Introduction

- Read this section, install ffuf and Seclists, then click on "Complete" buttons.
- I used automated tool and wordlist installer `HackiFy` to install those tools. Repo: https://github.com/ZishanAdThandar/HackiFy
## Basics

- Read this section properly, connect to the network with openvpn or start `AttackBox`.
- Click on `Start the Machine`.
- Used the given command `ffuf -u http://MACHINE_IP/NORAJ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt:NORAJ`, just replaced `seclists` location with `/opt/wordlist/SecLists/` as HackiFy install it inside `/opt/wordlist` directory.
- Question `What is the first file you found with a 200 status code?` Answer `favicon.ico`
## Finding pages and directories

- If we run first command `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-files-lowercase.txt` we can get some output.
- Question `What text file did you find?` Answer `robots.txt`
- If we run second command given `ffuf -u http://MACHINE_IP/indexFUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/web-extensions.txt` , we can get output.
- Question `What two file extensions were found for the index page?` Answer `php,phps`
- Again we need to run third given command `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt` and observe the output.
- Question `What page has a size of 4840?` Answer `about.php`
- If we run last given command, `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-directories-lowercase.txt`. We will get some directories.
- Question `How many directories are there?` Answer `4`
## Using filters

- Question `After applying the fc filter, how many results were returned?` Answer `11`. Got by observing output of command `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fc 403`.
- Question `After applying the mc filter, how many results were returned?` Answer `6` . Got by observing output of command `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-files-lowercase.txt -mc 200`
- Question `Which valuable file would have been hidden if you used -fc 403 instead of -fr?` Answer `wp-forum.phps`. Got by observing output difference between `-fc 403` command and command `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-files-lowercase.txt  -fr '/\..*'`
## Fuzzing parameters

- Terminate if any machine running and click on `Start Machine`. Also read this section.
- Question `What is the parameter you found?` Answer `id`. Got it from output of `ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?FUZZ=1' -c -w /opt/wordlist/SecLists/Discovery/Web-Content/burp-parameter-names.txt -fw 39`.
- Question `What is the highest valid id?` Answer `14`. Got it by running, `for i in {0..255}; do echo $i; done | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33`.
- Question `What is Dummy's password?` Answer `p@ssword`. Got it with command `ffuf -u http://MACHINE_IP/sqli-labs/Less-11/ -c -w /opt/wordlist/SecLists/Passwords/Leaked-Databases/hak5.txt -X POST -d 'uname=Dummy&passwd=FUZZ&submit=Submit' -fs 1435 -H 'Content-Type: application/x-www-form-urlencoded' `.
## Finding vhosts and subdomains

- Read the section properly, and mark it `Complete`.
## Proxifying ffuf traffic

- Read the section properly, and mark it `Complete`.
## Reviewing the options

- Observe output of `ffuf -h`.
- Question `How do you save the output to a markdown file (ffuf.md)?` Answer `-of md -o ffuf.md`
- Question `How do you re-use a raw http request file?` Answer `-request`
- Question `How do you strip comments from a wordlist?` Answer `-ic`
- Question `How would you read a wordlist from STDIN?` Answer `-w -`
- Question `How do you print full URLs and redirect locations?` Answer `-v`
- Question `What option would you use to follow redirects?` Answer `-r`
- Question `How do you enable colorized output?` Answer `-c`
## About the author

- Author details here, just click on `Complete` and done.

Author: [Zishan Ahamed Thandar](https://github.com/ZishanAdThandar/WriteUps/tree/main?tab=readme-ov-file#about-me)
