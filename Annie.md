# #1 introduction
<img width="646" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/3d64ade8-e774-4089-a488-a9393db178d6">

# #2 nmap

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
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
7070/tcp open  tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```bash
PORT      STATE         SERVICE
68/udp    open|filtered dhcpc
520/udp   open|filtered route
772/udp   open|filtered cycleserv2
5001/udp  open|filtered commplex-link
5353/udp  open|filtered zeroconf
16974/udp open|filtered unknown
17726/udp open|filtered unknown
18113/udp open|filtered unknown
18821/udp open|filtered unknown
18832/udp open|filtered unknown
19639/udp open|filtered unknown
19933/udp open|filtered unknown
20919/udp open|filtered unknown
21663/udp open|filtered unknown
23040/udp open|filtered unknown
30263/udp open|filtered unknown
31625/udp open|filtered unknown
34578/udp open|filtered unknown
34580/udp open|filtered unknown
46836/udp open|filtered unknown
49152/udp open|filtered unknown
53037/udp open|filtered unknown
```


# #3 user.txt
