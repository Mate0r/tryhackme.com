# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx empline.thm
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
sudo nmap -T4 -p- -sV empline.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
3306/tcp open  mysql   MySQL 5.5.5-10.1.48-MariaDB-0ubuntu0.18.04.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```




http://job.empline.thm/careers


we use a CVE to do a RCE

config.php of app
/* Database configuration. */
define('DATABASE_USER', 'james');
define('DATABASE_PASS', 'ng6pUFvsGNtw');
define('DATABASE_HOST', 'localhost');
define('DATABASE_NAME', 'opencats');


in mySQL we found these credentials
admin          | admin@testdomain.com | b67b5ecc5d8902ba59c65596e4c053ec |
| cats@rootadmin | 0                    | cantlogin                        |
| george         |                      | 86d0dfda99dbebc424eb4407947356ac |
| james          |                      | e53fbdb31890ff3bc129db0e27c473c9 
