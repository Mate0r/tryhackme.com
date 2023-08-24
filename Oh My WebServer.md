# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx ohmywebserver.thm
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
sudo nmap -T4 -p- -sV ohmywebserver.thm
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

# HTTP (port 31331)
On the homepage, we learn there is 4 employees
```bash
Paradox - Our lead web designer, Paradox can help you create your dream website from the ground up\
Elf - Overpass' newest intern, Elf. Elf helps maintain the webservers day to day to keep your site running smoothly and quickly.\
MuirlandOracle - HTTPS and networking specialist. Muir's many years of experience and enthusiasm for networking keeps Overpass running, and your sites, online all of the time.\
NinjaJc01 - James started Overpass, and keeps the business side running. If you have pricing questions or want to discuss how Overpass can help your business, reach out to him!\
```
we found /backups/ url with a backup.zip file
in this backup file, there is a gpg file and a key file (used to decrypt the gpg
we decrypt with
```bash
gpg --import priv.key
gpg -d CustomerDetails.gpg 
```

<img width="702" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/c9b6c294-9c53-4fca-b7f0-f66aed812f0b">

paradox:ShibesAreGreat123\
0day:OllieIsTheBestDog\
muirlandoracle:A11D0gsAreAw3s0me


with this account, i access to ftp paradox:ShibesAreGreat123

```bash
root:x:0:0:root:/root:/bin/bash
james:x:1000:1000:James:/home/james:/bin/bash
paradox:x:1001:1001::/home/paradox:/bin/bash
```

we can use the previously found credentials of paradox to do a su and become paradox : paradox:ShibesAreGreat123
```bash
bash-4.4$ su paradox
Password: 
[paradox@localhost /]$
```

we see there is /usr/bin/crontab with suid bit\
we'll try to add a job executed by root


/home/paradox/.local/bin:/home/paradox/bin:/home/paradox/.local/bin:/home/paradox/bin:/usr/local/bin:/usr/bin
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin

pspy64


tcp     LISTEN   0        128              0.0.0.0:111            0.0.0.0:*                                                                                  
tcp     LISTEN   0        128              0.0.0.0:20048          0.0.0.0:*     
tcp     LISTEN   0        64               0.0.0.0:43221          0.0.0.0:*     
tcp     LISTEN   0        128              0.0.0.0:22             0.0.0.0:*     
tcp     LISTEN   0        128              0.0.0.0:51255          0.0.0.0:*     
tcp     LISTEN   0        64               0.0.0.0:2049           0.0.0.0:*     
tcp     LISTEN   0        64                  [::]:43563             [::]:*     
tcp     LISTEN   0        128                 [::]:111               [::]:*     
tcp     LISTEN   0        128                 [::]:20048             [::]:*     
tcp     LISTEN   0        128                    *:80                   *:*     
tcp     LISTEN   0        128                 [::]:58355             [::]:*     
tcp     LISTEN   0        32                     *:21                   *:*     
tcp     LISTEN   0        128                 [::]:22                [::]:*     
tcp     LISTEN   0        64                  [::]:2049              [::]:*


╔══════════╣ Analyzing NFS Exports Files (limit 70)
Connected NFS Mounts:                                                                                                                                        
nfsd /proc/fs/nfsd nfsd rw,relatime 0 0
sunrpc /var/lib/nfs/rpc_pipefs rpc_pipefs rw,relatime 0 0
-rw-r--r--. 1 root root 54 Nov 18  2020 /etc/exports
/home/james *(rw,fsid=0,sync,no_root_squash,insecure)


thm{3693fc86661faa21f16ac9508a43e1ae}
