r# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx catpictures2.thm
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
sudo nmap -T4 -p- -sV catpictures2.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    nginx 1.4.6 (Ubuntu)
222/tcp  open  ssh     OpenSSH 9.0 (protocol 2.0)
1337/tcp open  waste?
3000/tcp open  ppp?
8080/tcp open  http    SimpleHTTPServer 0.6 (Python 3.6.9)
```

# HTTP (port 80)
it's a webapp running called Lychee in version 3.1.1\
there is a robots.txt 
```
User-agent: *
Disallow: /data/
Disallow: /dist/
Disallow: /docs/
Disallow: /php/
Disallow: /plugins/
Disallow: /src/
Disallow: /uploads/
```

there is a .htaccess readable
```
Options -Indexes

# ---
# Uncomment these lines to change PHP parameters if you are using the PHP Apache module
# ---
#<IfModule mod_php5.c>
#	php_value max_execution_time 200
#	php_value post_max_size 200M
#	php_value upload_max_size 200M
#	php_value upload_max_filesize 20M
#	php_value max_file_uploads 100
#</IfModule>

# ---
# Uncomment these lines when you want to allow access to the Lychee API from different origins
# ---
#Header add Access-Control-Allow-Origin "*"
#Header add Access-Control-Allow-Headers "origin, x-requested-with, content-type"
#Header add Access-Control-Allow-Methods "PUT, GET, POST, DELETE, OPTIONS"
```

http://catpictures2.thm/php/ -> "Error: No API function specified!"


# OliveTin (port 1337)
there is a user bismuth\
there is a call to a script that doens't exist on /opt/backupScript.sh


# Gitea (port 3000)
there is a webapp named Gitea that is running on version 1.17.3

http://catpictures2.thm:3000/v2/token \
http://catpictures2.thm:3000/v2/.svn/entries \
http://catpictures2.thm:3000/v2/ \
http://catpictures2.thm:3000/v2/.git/HEAD \
http://catpictures2.thm:3000/v2/_vti_bin/_vti_aut/author.dll \
http://catpictures2.thm:3000/v2/_vti_bin/shtml.dll \
http://catpictures2.thm:3000/v2/_vti_bin/_vti_adm/admin.dll \
http://catpictures2.thm:3000/v2/CVS/Root \
http://catpictures2.thm:3000/v2/CVS/Entries \
http://catpictures2.thm:3000/v2/CVS/Repository \

[with someenumeration with feroxbuster, we found an url we give us a token for v2 (](http://catpictures2.thm:3000)/v2/token)
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2OTMwNDc2MzYsIm5iZiI6MTY5Mjk2MTIzNiwiVXNlcklEIjotMX0.HtzFlMPU3VnpAIi2sFnNCO-rCwSpLeKlPmEsmJfHR2w

localhost
root
e7128b67d9e7
root
127.0.0.1
root
root
localhost
debian-sys-maint*015CC53362CDFB4ECD2CE756876D4A235EBE8E75
localhost
lychee*4E1E4103B94B2548A6A11F31EFEFF7910DDB13D3
