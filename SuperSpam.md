# introduction
<img width="645" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/32f1a109-5e3a-42bb-959f-a8eaf3ebf39a">

# nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx superspam.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- superspam.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.29
4012/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
4019/tcp open  ftp     vsftpd 3.0.3
5901/tcp open  vnc     VNC (protocol 3.8)
6001/tcp open  X11     (access denied)
Service Info: OSs: Linux, Unix; CPE: cpe:/o:linux:linux_kernel
```

# user.txt

## FTP (port 22)
there is a username : adam

in the note.txt, we can see the attack is occuring at midnight (the 13th)
an existing blog (wordpress ?) has been removed (the 15th)

super-spam

SSID Motocplus
there is the 4 handshake in the wiresharl EAPOL
