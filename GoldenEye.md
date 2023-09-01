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


username: xenia
password: RCP90rulez!

severnaya-station.com/gnocertdir

doak:goat

username: dr_doak
password: 4England!

admin:xWinter1995x!


$CFG->dbhost    = 'localhost';
$CFG->dbname    = 'moodle';
$CFG->dbuser    = 'moodle';
$CFG->dbpass    = 'trevelyan006x';


id | username |             password             
----+----------+----------------------------------
  1 | guest    | aca21c6dbd0538a171ff16550b873d70
  2 | admin    | de51800b0404d41fcb51203f1e3e524a
  3 | boris    | efaf365b88dee2fc2029ff20674658a7
  4 | natalya  | 7a442035ba13c5d22e1e163e2117eb0d
  6 | xenia    | 116672a0e281b6aa277ef78f53a5f6f9
  5 | dr_doak  | 488e0292fac2386d877e80f2e3a203bf


  boris
  doak
  natalya
  
