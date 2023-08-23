# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx anonymous.thm
```

# Enumeration

we do a nmap with some options :\
\
-T4 is to have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV anonymous.thm
```

we got these open ports
```bash
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.0.8 or later
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: Host: ANONYMOUS; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# FTP (port 21)
we'll try to connect to ftp server as anonymous
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

we can now list the files in FTP
```bash
ftp> ls -la
229 Entering Extended Passive Mode (|||44253|)
150 Here comes the directory listing.
drwxr-xr-x    3 65534    65534        4096 May 13  2020 .
drwxr-xr-x    3 65534    65534        4096 May 13  2020 ..
drwxrwxrwx    2 111      113          4096 Jun 04  2020 scripts
226 Directory send OK.
ftp> cd scripts
250 Directory successfully changed.
ftp> ls -la
229 Entering Extended Passive Mode (|||22658|)
150 Here comes the directory listing.
drwxrwxrwx    2 111      113          4096 Jun 04  2020 .
drwxr-xr-x    3 65534    65534        4096 May 13  2020 ..
-rwxr-xrwx    1 1000     1000          314 Jun 04  2020 clean.sh
-rw-rw-r--    1 1000     1000         1505 Aug 23 09:39 removed_files.log
-rw-r--r--    1 1000     1000           68 May 12  2020 to_do.txt
226 Directory send OK.
```
