# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx ultratech.thm
```

# Enumeration

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV ultratech.thm
```

we got these open ports
```bash
21/tcp    open  ftp     vsftpd 3.0.3
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
8081/tcp  open  http    Node.js Express framework
31331/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
```

# FTP (port 21)
we'll try to connect to the FTP server with anonymous account
```bash
$ ftp ultratech.thm                                                                                    
Connected to ultratech.thm.
220 (vsFTPd 3.0.3)
Name (ultratech.thm:kali): anonymous
331 Please specify the password.
Password: 
530 Login incorrect.
```

it's not working, let's move to next

# Node.js (port 8081)
we found that there is an api running on port 8081 (custom one ?) : UltraTech API v0.1.3\
we do a feroxbuster and find some interisting URL :
http://ultratech.thm:8081/auth
http://ultratech.thm:8081/ping

in auth, if we send a GET request with login and password as query parameters, we have invalid credentials\
the /ping/ url is interisting : we can set a query parameter ip and it pings the ip\
maybe we can inject one command

# HTTP (port 31331)
we found a robots.txt
```bash
Allow: *
User-Agent: *
Sitemap: /utech_sitemap.txt
```

if we visit the /utech_sitemap.txt
```bash
/
/index.html
/what.html
/partners.html
```
