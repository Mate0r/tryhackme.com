# Introduction

We first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx 0day.thm
```

# Enumeration

We do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV 0day.thm
```

We got this result
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# HTTP (Port 80)
We enumarate the website with feroxbuster and found some interestings folders

```bash
feroxbuster -u http://0day.thm/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

```bash
301      GET        9l       28w      305c http://0day.thm/cgi-bin => http://0day.thm/cgi-bin/
301      GET        9l       28w      301c http://0day.thm/img => http://0day.thm/img/
200      GET       78l      138w     1114c http://0day.thm/css/main.css
200      GET        7l       11w      156c http://0day.thm/js/main.js
301      GET        9l       28w      305c http://0day.thm/uploads => http://0day.thm/uploads/
200      GET      423l     2430w   194997c http://0day.thm/img/avatar.png
200      GET       42l      136w     3025c http://0day.thm/
301      GET        9l       28w      303c http://0day.thm/admin => http://0day.thm/admin/
301      GET        9l       28w      301c http://0day.thm/css => http://0day.thm/css/
301      GET        9l       28w      300c http://0day.thm/js => http://0day.thm/js/
301      GET        9l       28w      304c http://0day.thm/backup => http://0day.thm/backup/
301      GET        9l       28w      304c http://0day.thm/secret => http://0day.thm/secret/
```

At url /backup/ we found a private SSH key
<img width="570" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/f73e3c99-906a-4a4a-8686-8b5fdb7be74e">
