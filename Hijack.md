# #1 Introduction
<img width="644" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/a95db165-a8d1-4830-bd51-c59464c08c96">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx hijack.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- hijack.thm
```

we got these open ports
```bash
PORT      STATE SERVICE  VERSION
21/tcp    open  ftp      vsftpd 3.0.3
22/tcp    open  ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http     Apache httpd 2.4.18 ((Ubuntu))
111/tcp   open  rpcbind  2-4 (RPC #100000)
2049/tcp  open  nfs      2-4 (RPC #100003)
40030/tcp open  mountd   1-3 (RPC #100005)
42026/tcp open  nlockmgr 1-4 (RPC #100021)
43165/tcp open  mountd   1-3 (RPC #100005)
44322/tcp open  mountd   1-3 (RPC #100005)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

logout.php
config.php
administration.php
login.php
signup.php
navbar.php
index.php

$ showmount -e hijack.thm
Export list for hijack.thm:
/mnt/share *

sudo mount -t nfs hijack.thm:/mnt/share /mnt/hijack


ftpuser:W3stV1rg1n14M0un741nM4m4

admin:uDh3jCQsdcuLhjVkAy5x
