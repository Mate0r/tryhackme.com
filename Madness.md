# #1 Introduction

![image](https://github.com/MaTe0r/tryhackme.com/assets/94843357/9b1204af-d6df-4aed-a8c1-2cd43d4954be)

# #2 Add a host

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx madness.thm
```

# #3 Nmap

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-sV is to scan the version\
-p- is to scan all the ports\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- madness.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #4 User.txt

Tomcat (port 8080)
i tried to go to host manager by using default credentials that are tomcat:s3cret and it worked.
let's now update a .war manager app that will give us a reverse shell



so we used this exploit that give us the ability to do RCE\
https://github.com/p0dalirius/Tomcat-webshell-application

so we so a python rev shell, upload it to /tmp and execute it

# #4 Root.txt
we can now edit in the /home/jack the id.sh\
this file is called every minutes by a root cron and we can edit it\
so we put a rev shell code in it and we are now root, bingo


