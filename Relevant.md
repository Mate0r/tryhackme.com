# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx relevant.thm
```

# Enumeration

we do a nmap with some options :\
\
-T4 is to have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV relevant.thm
```

we got these open ports
```bash
PORT    STATE SERVICE     VERSION
PORT     STATE SERVICE        VERSION
80/tcp   open  http           Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
135/tcp  open  msrpc          Microsoft Windows RPC
139/tcp  open  netbios-ssn    Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds   Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp open  ms-wbt-server?
49663/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
```

# HTTP (port 80)

# RPC Remote Procedure Call (port 135)

# Microsoft DS SMB (port 139 and 445)

cat passwords.txt               
[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk

Bob - !P@$$W0rD!123
Bill - Juw4nnaM4n420696969!$$$

# RDP or Remote Desktop Protocol (port 3389)
