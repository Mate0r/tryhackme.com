# #1 introduction
<img width="658" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/f8ca4592-6f63-41ad-bafa-3a6e8591f10a">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx agentsudo.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- agentsudo.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## FTP (port 21)
we tried first to connect as anonymous in FTP (port 21) but without success

<img width="703" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/6228cdfb-8816-4a8b-8ebb-58cdf19442c3">

## HTTP (port 80)
we got this default page on the webserver

<img width="669" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/08745c5a-4b8b-42ad-b17c-4298d2bbf958">

let's uses ffuf to fuzz for

<img width="1319" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/e0298a10-11c3-41ce-b90d-fc8d9f5607a9">



let's use hydra to try to bruteforce an FTP account with username chris

<img width="1332" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/edc6a065-f2dd-4395-adbd-e36058a5ba3e">


we found a password crystal associated to chris

<img width="644" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/de9244f8-92cf-47c7-8158-f2fab9f2bbff">

<img width="679" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/71f4fdbf-da07-404a-8334-fe9f3155c7af">

<img width="842" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/4ba1e35a-d462-4a8a-9055-5d083e028b46">

james:hackerrules!



