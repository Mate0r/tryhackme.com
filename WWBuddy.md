# #1 introduction
<img width="649" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/684894e8-553b-4b59-acae-e28249a4293e">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx wwbuddy.thm
```

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- wwbuddy.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


# #3 user.txt

http://wwbuddy.thm/admin (nothing after enumerating)

http://wwbuddy.thm/api (the only available directory is /messages/)
-> http://wwbuddy.thm/api/messages/





http://wwbuddy.thm/login/
http://wwbuddy.thm/chat.php
http://wwbuddy.thm/register
http://wwbuddy.thm/change
http://wwbuddy.thm/profile


we can enumerate users with hydra :
- Roberto : 04/14/1995
- Henry : 12/12/1212

Roberto:yVnocsXsf%X68wf
