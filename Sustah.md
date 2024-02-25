# #1 Introduction
<img width="652" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/fa9a7d72-156c-4c58-bb31-81850b9f13a0">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx sustah.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- sustah.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
8085/tcp open  http    Gunicorn 20.0.4
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

sustah.thm:8085/ping -> PONG


