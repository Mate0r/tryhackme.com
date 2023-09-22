# #1 Introduction
<img width="648" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/1ea9ba7e-72e4-4e4f-877f-ab928f63fc64">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx lookingglass.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- lookingglass.thm
```

we got these open ports
```bash

```

# #3 user.txt

ssh 10.10.114.165 -p 12849 -oHostKeyAlgorithms=+ssh-rsa
we found the port is 12849

