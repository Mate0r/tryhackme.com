# #1 Introduction
<img width="655" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/f50b9599-5050-4764-abdb-adb582c0570b">

# #2 Nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx theserverfromhell.thm
```

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- theserverfromhell.thm
```

we got these open ports
```bash

```


