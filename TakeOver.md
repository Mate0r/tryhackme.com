# #1 Introduction
<img width="647" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/e8fbd89e-41c2-430d-8e04-67ef8f17dfc6">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx futurevera.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- futurevera.thm
```

we got these open ports
```bash
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http     Apache httpd 2.4.41 ((Ubuntu))
443/tcp open  ssl/http Apache httpd 2.4.41
Service Info: Host: futurevera.thm; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

