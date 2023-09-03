# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx toc2.thm
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
sudo nmap -T4 -p- -sV toc2.thm
```

we got these open ports
```bash

```


we found on home page credentials for cms : cmsmsuser:devpass
<img width="879" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/79ba6886-c39b-49d1-be23-3ad7faf6ea41">

we found in robots.txt
```
User-agent: *
Disallow: /cmsms/cmsms-2.1.6-install.php
 
Note to self:
Tommorow, finish setting up the CMS, and that database, cmsmsdb, so the site's ready by Wednesday.
```

let's explore this cms that seems to be CMS Made Simple so the version is 2.1.6 ?\
so we navigate to the install php file of the cms : /cmsms/cmsms-2.1.6-install.php\
we create the following admin account : admin/admin@toc2.thm with password "password"

we found that CMS made simple 2.1.6 has a remove code execution :
<img width="1260" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/4aa4b06e-44be-4c12-8500-ef86a65a9b11">



we have a foothold with CVE-
we specify a timezone with a command injection that we can use as parameter by accessing config.php file

so now we are in frank's home directory and wee see the file new_machine.txt
```
www-data@toc:/home/frank$ cat new_machine.txt 
I'm gonna be switching computer after I get this web server setup done. The inventory team sent me a new Thinkpad, the password is "password". It's funny that the default password for all the work machines is something so simple...Hell I should probably change this one from it, ah well. I'm switching machines soon- it can wait.
```

we try to connect as ssh with frank and password "password" and it works ! \

# ROOT
we have a folder root_access with a SUID binary and even it's source code.

we could abuse the binary by using a TOCTOU vulnerability
the access function check for the real UID and GID of the user who execute the binary so it's always frank
but the open function doesn't and use the effective UID and GID so if between the call of access() and open() function, we edit a link to a protected file, we can read it by luck

Root Credentials:  root:aloevera

I used this exploit to got it : https://github.com/davidenetti/TOCTOU_Vulnerability/blob/main/attackrace.c
