# #1 Introduction

![image](https://github.com/MaTe0r/tryhackme.com/assets/94843357/c7fee9b7-2f20-4ab3-9474-c03a68a9c997)


# #2 Add a host

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx thompson.thm
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
sudo nmap -T4 -sV -p- thompson.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
8080/tcp open  http    Apache Tomcat 8.5.5
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


