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
- [[Severity 3] Sensitive Data Exposure (Challenge)](#severity-3-sensitive-data-exposure-challenge)
- [[Severity 4] XML External Entity](#severity-4-xml-external-entity)
- [[Severity 4] XML External Entity - eXtensible Markup Language](#severity-4-xml-external-entity---extensible-markup-language)
- [[Severity 4] XML External Entity - DTD](#severity-4-xml-external-entity---dtd)
- [[Severity 4] XML External Entity - XXE Payload](#severity-4-xml-external-entity---xxe-payload)
- [[Severity 4] XML External Entity - Exploiting](#severity-4-xml-external-entity---exploiting)
- [[Severity 5] Broken Access Control](#severity-5-broken-access-control)
- [[Severity 5] Broken Access Control (IDOR Challenge)](#severity-5-broken-access-control-idor-challenge)
- [[Severity 6] Security Misconfiguration](#severity-6-security-misconfiguration)
- [[Severity 7] Cross-site Scripting](#severity-7cross-site-scripting)
- [[Severity 8] Insecure Deserialization](#severity-8-insecure-deserialization)
- [[Severity 8] Insecure Deserialization - Objects](#severity-8-insecure-deserialization---objects)
- [[Severity 8] Insecure Deserialization - Deserialization](#severity-8-insecure-deserialization---deserialization)
- [[Severity 8] Insecure Deserialization - Cookies](#severity-8-insecure-deserialization---cookies)
- [[Severity 8] Insecure Deserialization - Cookies Practical](#severity-8-insecure-deserialization---cookies-practical)
- [[Severity 8] Insecure Deserialization - Code Execution](#severity-8-insecuredeserialization---code-execution)
- [[Severity 9] Components With Known Vulnerabilities - Intro](#severity-9-components-with-known-vulnerabilities---intro)
- [[Severity 9] Components With Known Vulnerabilities - Exploit](#severity-9-components-with-known-vulnerabilities---exploit)
- [[Severity 9] Components With Known Vulnerabilities - Lab](#severity-9-components-with-known-vulnerabilities---lab)
- [[Severity 10] Insufficient Logging and Monitoring](#severity-10-insufficient-logging-and-monitoring)
- [What Next?](#what-next)

Room Link: https://tryhackme.com/r/room/owasptop10

## Tools
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
1. Start Machine and get Target IP from "Target Machine Information". Now, open http://machine_ip/evilshell.php.
2. Now, type commands and submit. You can see output below.
3. Question `What strange text file is in the website root directory?` Answer `drpepper.txt`. Running `ls` command will show this strange text file.
4. Question `How many non-root/non-service/non-daemon users are there?` Answer `0`. Running `cat /etc/passwd` will show.
5. Question `What user is this app running as?` Answer `www-data`. Used command `whoami`.
6. Question `What is the user's shell set as?` Answer `/usr/sbin/nologin`. Command used `getent passwd www-data` or `cat /etc/passwd |grep www-data`.
## [Severity 2] Broken Authentication
## [Severity 2] Broken Authentication Practical
## [Severity 3] Sensitive Data Exposure (Introduction)
## [Severity 3] Sensitive Data Exposure (Supporting Material 1)
## [Severity 3] Sensitive Data Exposure (Challenge)
## [Severity 4] XML External Entity
## [Severity 4] XML External Entity - eXtensible Markup Language
## [Severity 4] XML External Entity - DTD
## [Severity 4] XML External Entity - XXE Payload
## [Severity 4] XML External Entity - Exploiting
## [Severity 5] Broken Access Control
## [Severity 5] Broken Access Control (IDOR Challenge)
## [Severity 6] Security Misconfiguration
## [Severity 7] Cross-site Scripting
## [Severity 8] Insecure Deserialization
## [Severity 8] Insecure Deserialization - Objects
## [Severity 8] Insecure Deserialization - Deserialization
## [Severity 8] Insecure Deserialization - Cookies
## [Severity 8] Insecure Deserialization - Cookies Practical
## [Severity 8] Insecure Deserialization - Code Execution
## [Severity 9] Components With Known Vulnerabilities - Intro
## [Severity 9] Components With Known Vulnerabilities - Exploit
## [Severity 9] Components With Known Vulnerabilities - Lab
## [Severity 10] Insufficient Logging and Monitoring
## What Next?



Author: Zishan Ahamed Thandar
