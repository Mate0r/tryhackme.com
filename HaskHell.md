# #1 Introduction
![image](https://github.com/MaTe0r/tryhackme.com/assets/94843357/24f08b56-9740-44b2-b115-dca148614fa4)

# #2 Nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx haskhell.thm
```

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- haskhell.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
5001/tcp open  http    Gunicorn 19.7.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


