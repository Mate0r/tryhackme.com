# #1 Introduction
<img width="646" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/e998120e-2791-4ea6-81ec-d45726ecb49f">

# #2 Nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx biteme.thm
```

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- biteme.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


