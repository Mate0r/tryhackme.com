# Tech_Supp0rt: 1
https://tryhackme.com/room/techsupp0rt1

## Nmap
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-01 16:12 CEST  
Nmap scan report for 10.10.22.244  
Host is up (0.041s latency).  
Not shown: 996 closed tcp ports (conn-refused)  
PORT    STATE SERVICE     VERSION  
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:  
|   2048 10:8a:f5:72:d7:f9:7e:14:a5:c5:4f:9e:97:8b:3d:58 (RSA)  
|   256 7f:10:f5:57:41:3c:71:db:b5:5b:db:75:c9:76:30:5c (ECDSA)  
|_  256 6b:4c:23:50:6f:36:00:7c:a6:7c:11:73:c1:a8:60:0c (ED25519)  
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))  
|_http-server-header: Apache/2.4.18 (Ubuntu)  
|_http-title: Apache2 Ubuntu Default Page: It works  
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)  
445/tcp open              Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)  
Service Info: Host: TECHSUPPORT; OS: Linux; CPE: cpe:/o:linux:linux_kernel  

## Samba server
``` mount_smbfs -N //guest@10.10.22.244/websvr mnt/ ```

