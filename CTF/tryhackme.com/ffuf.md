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

Room Link: https://tryhackme.com/r/room/ffuf

## Tools
1. [HackiFy](https://github.com/ZishanAdThandar/HackiFy)
2. [ffuf](https://github.com/ffuf/ffuf)
3. [SecLists](https://github.com/danielmiessler/SecLists)

## Introduction
1. Read this section, install ffuf and Seclists, then click on "Complete" buttons.
2. I used automated tool and wordlist installer `HackiFy` to install those tools. Repo: https://github.com/ZishanAdThandar/HackiFy
## Basics
1. Read this section properly, connect to the network with openvpn or start `AttackBox`.
2. Click on `Start the Machine`.
3. Used the given command `ffuf -u http://MACHINE_IP/NORAJ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt:NORAJ`, just replaced `seclists` location with `/opt/wordlist/SecLists/` as HackiFy install it inside `/opt/wordlist` directory.
4. Question `What is the first file you found with a 200 status code?` Answer `favicon.ico`
## Finding pages and directories
1. If we run first command `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-files-lowercase.txt` we can get some output.
2. Question `What text file did you find?` Answer `robots.txt`
3. If we run second command given `ffuf -u http://MACHINE_IP/indexFUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/web-extensions.txt` , we can get output.
4. Question `What two file extensions were found for the index page?` Answer `php,phps`
5. Again we need to run third given command `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt` and observe the output.
6. Question `What page has a size of 4840?` Answer `about.php`
7. If we run last given command, `ffuf -u http://MACHINE_IP/FUZZ -w /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-directories-lowercase.txt`. We will get some directories.
8. Question `How many directories are there?` Answer `4`
## Using filters
## Fuzzing parameters
## Finding vhosts and subdomains
## Proxifying ffuf traffic
## Reviewing the options
## About the author

Author: [Zishan Ahamed Thandar](https://github.com/ZishanAdThandar/WriteUps/tree/main?tab=readme-ov-file#about-me)
