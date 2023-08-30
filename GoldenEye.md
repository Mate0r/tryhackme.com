# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx goldeneye.thm
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
sudo nmap -T4 -p- -sV goldeneye.thm
```

we find these open ports
```bash
PORT      STATE SERVICE  VERSION
25/tcp    open  smtp     Postfix smtpd
80/tcp    open  http     Apache httpd 2.4.7 ((Ubuntu))
55006/tcp open  ssl/pop3 Dovecot pop3d
55007/tcp open  pop3     Dovecot pop3d
```


# SMTP (port 25)



# HTTP (port 80)
we have a basic html page with a "terminal.js" script that display text\
we have an url /sev-home/ who needs an authentication\
we found credentials for Boris/boris in terminal.js file with a entity html encoded password : &#73;&#110;&#118;&#105;&#110;&#99;&#105;&#98;&#108;&#101;&#72;&#97;&#99;&#107;&#51;&#114;\
it gives us this password once decoded : InvincibleHack3r


we access the restricted page and found there may be a second user named natalya on the server :
Qualified GoldenEye Network Operator Supervisors: 
Natalya
Boris


# POP3 (port 55006 and 55007)
let's see the 55007 since it's not SSL so we can test it with telnet directly

we found a boris user with password secret1!
