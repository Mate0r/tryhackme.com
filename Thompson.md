# #1 Introduction

![image](https://github.com/MaTe0r/tryhackme.com/assets/94843357/c7fee9b7-2f20-4ab3-9474-c03a68a9c997)


# Add a host

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx thompson.thm
```

# Enumeration

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
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.4 (protocol 2.0)
12340/tcp open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
```
