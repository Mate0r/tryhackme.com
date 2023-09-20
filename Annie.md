# #1 Introduction
<img width="646" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/3d64ade8-e774-4089-a488-a9393db178d6">

# #2 Nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx annie.thm
```

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- annie.thm
```

we got these open ports
```bash

```
