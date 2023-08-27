# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx smaggrotto.thm
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
sudo nmap -T4 -p- -sV smaggrotto.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


<a>To: netadmin@smag.thm</a>
					<a>Cc: uzi@smag.thm</a>
					<!-- <a>Bcc: trodd@smag.thm</a> -->
					<a>From: jake@smag.thm</a>

     in pcac file, we found a subdomain : development.smag.thm

     /login.php : username=helpdesk&password=cH4nG3M3_n0w


     we do a reverse shell by entering command
     


     we found /opt/.backups/jake_id_rsa.pub.backup

     with sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
     
