# Introduction

We first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx simplectf.thm
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
sudo nmap -T4 -p- -sV simplectf.thm
```

We got this result
```bash
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

# FTP (port 21)
let's try to connect as anonymous
```bash
$ ftp simplectf.thm
Connected to simplectf.thm.
220 (vsFTPd 3.0.3)
Name (simplectf.thm:kali): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```

It works ! Not let's list the file with a ls -la
```bash
ftp> ls -la
229 Entering Extended Passive Mode (|||47710|)
```

It's not working, it says it is in passive mode so we need to disable it by doing the command PASSIVE
```bash
^C
receive aborted. Waiting for remote to finish abort.
ftp> passive
Passive mode: off; fallback to active mode: off.
ftp> ls -la
200 EPRT command successful. Consider using EPSV.
150 Here comes the directory listing.
drwxr-xr-x    3 ftp      ftp          4096 Aug 17  2019 .
drwxr-xr-x    3 ftp      ftp          4096 Aug 17  2019 ..
drwxr-xr-x    2 ftp      ftp          4096 Aug 17  2019 pub
226 Directory send OK.
```

If we go in pub folder, we found a file name "ForMitch.txt" who contains this hint
```
Dammit man... you'te the worst dev i've seen. You set the same pass for the system user, and the password is so weak... i cracked it in seconds. Gosh... what a mess!
```

Seems we could try a bruteforce attack on the SSH port with the mitch user\
Let's see the website and we'll try further

# HTTP (port 80)
Let's see the website !\
First, we found a robots.txt
```
#
# "$Id: robots.txt 3494 2003-03-19 15:37:44Z mike $"
#
#   This file tells search engines not to index your CUPS server.
#
#   Copyright 1993-2003 by Easy Software Products.
#
#   These coded instructions, statements, and computer programs are the
#   property of Easy Software Products and are protected by Federal
#   copyright law.  Distribution and use rights are outlined in the file
#   "LICENSE.txt" which should have been included with this file.  If this
#   file is missing or damaged please contact Easy Software Products
#   at:
#
#       Attn: CUPS Licensing Information
#       Easy Software Products
#       44141 Airport View Drive, Suite 204
#       Hollywood, Maryland 20636-3111 USA
#
#       Voice: (301) 373-9600
#       EMail: cups-info@cups.org
#         WWW: http://www.cups.org
#

User-agent: *
Disallow: /


Disallow: /openemr-5_0_1_3 
#
# End of "$Id: robots.txt 3494 2003-03-19 15:37:44Z mike $".
#
```

This could be interesting, maybe there is an user named mike ? in addition of mitch ?\
We do a feroxbuster to enumarate directories and found a /simple directory

```bash
$ feroxbuster -u http://simplectf.thm/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

```bash
301      GET        9l       28w      315c http://simplectf.thm/simple => http://simplectf.thm/simple/
301      GET        9l       28w      321c http://simplectf.thm/simple/admin => http://simplectf.thm/simple/admin/
```

So we figure out by visiting this url that there is an webapp named CMS Made Simple in v2.2.8
<img width="807" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/49b86b59-a666-4f90-baec-db916a14d9f9">

In googling the version, we found that this is vulnerable to a RCE (https://www.exploit-db.com/exploits/46635)


