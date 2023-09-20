# #1 introduction
<img width="658" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/f8ca4592-6f63-41ad-bafa-3a6e8591f10a">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx agentsudo.thm
```

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- agentsudo.thm
```

we got these open ports
```bash

```


# #3 user.txt
