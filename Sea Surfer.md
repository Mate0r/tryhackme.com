# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx seasurf.thm
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
sudo nmap -T4 -p- -sV seasurf.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# HTTP (port 80)
we found a wordpress app v5.9.3

we have a robots.txt
```
User-agent: *
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php

Sitemap: http://seasurfer.thm/wp-sitemap.xml
```

by looking to the sitemap, we have an user named kyle
(Maya and Brandon ?)

sales@seasurfer.thm
support@seasurfer.thm

intrenal.seasurfer.thm : we found that domain in comment section (maybe misspelled, more internal ?)
after further investigation, it is well internal and not intrenal

http://internal.seasurfer.thm/invoices/
http://internal.seasurfer.thm/maintenance/

the page internal generate a invoice with the library wkhtmltopdf 0.12.5


define( 'DB_NAME', 'wordpress' );
/** Database username */
define( 'DB_USER', 'wordpressuser' );
/** Database password */
define( 'DB_PASSWORD',
'coolDataTablesMan' );
/** Database hostname */
define( 'DB_HOST', 'localhost' );


INSERT INTO `wp_users` (`ID`, `user_login`, `user_pass`, `user_nicename`, `user_email`, `user_url`, `user_registered`, `user_activation_key`, `user_status`, `display_name`) VALUES
(1,	'kyle',	'$P$BuCryp52DAdCRIcLrT9vrFNb0vPcyi/',	'kyle',	'kyle@seasurfer.thm',	'http://seasurfer.thm',	'2022-04-17 19:32:10',	'',	0,	'kyle');


jenny4ever       (kyle)

we edit the 404 to get a rev shell

we found a password in the logs



