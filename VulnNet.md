# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx vulnnet.thm
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
sudo nmap -T4 -p- -sV vulnnet.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# HTTP (port 80)
by scanning vhosts, we found a subdomain broadcast" 
broadcast.vulnnet.thm


┌──(parallels㉿kali)-[~/hacking/thm/VulnNet]
└─$ john .htpasswd --wordlist=/usr/share/wordlists/rockyou.txt 
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long"
Use the "--format=md5crypt-long" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 128/128 ASIMD 4x2])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
9972761drmfsls   (developers)     
1g 0:00:00:30 DONE (2023-09-13 21:30) 0.03231g/s 69825p/s 69825c/s 69825C/s 9978..9972727
Use the "--show" option to display all of the cracked passwords reliably
Session completed.


//Database Name
        $DBNAME = 'VulnNet';
        //Database Username
        $DBUSER = 'admin';
        //Database Password
        $DBPASS = 'VulnNetAdminPass0990';



oneTWO3gOyac     (id_rsa)


we can abuse /var/opt/backpserver.sh car il fait un tar wildcard (*)

