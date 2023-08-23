# Introduction

We first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx anonymous.thm
```

# Enumeration

We do a nmap with some options :\
\
-T4 is to have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV anonymous.thm
```

We got these open ports
```bash
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.0.8 or later
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: Host: ANONYMOUS; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# FTP (port 21)
We'll try to connect to ftp server as anonymous
```bash
┌──(kali㉿kali)-[~]
└─$ ftp anonymous.thm
Connected to anonymous.thm.
220 NamelessOne's FTP Server!
Name (anonymous.thm:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```
