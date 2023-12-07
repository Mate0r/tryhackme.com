# #1 Introduction
<img width="648" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/1355ba96-9793-43d4-a4cb-b59a39e88649">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx coldvvars.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- coldvvars.thm
```

we got these open ports
```bash
PORT     STATE SERVICE     VERSION
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
8080/tcp open  http        Apache httpd 2.4.29 ((Ubuntu))
8082/tcp open  http        Node.js Express framework
Service Info: Host: INCOGNITO
```

# #3 user.txt

## Samba (port 139 et 445)


