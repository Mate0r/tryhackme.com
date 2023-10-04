# #1 Introduction
<img width="646" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/4b445495-95f2-4bab-89ed-f9c99b8ec0fc">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx vulnnetnode.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- vulnnetnode.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
8080/tcp open  http    Node.js Express framework
```

# #3 user.txt

## HTTP (port 80)

