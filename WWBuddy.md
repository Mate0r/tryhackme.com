# #1 introduction
<img width="649" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/684894e8-553b-4b59-acae-e28249a4293e">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx wwbuddy.thm
```

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- wwbuddy.thm
```

we got these open ports
```bash

```


# #3 user.txt