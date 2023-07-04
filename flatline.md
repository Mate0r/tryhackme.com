# Flatline
https://tryhackme.com/room/flatline

## Nmap
PORT     STATE SERVICE          VERSION
3389/tcp open  ms-wbt-server    Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: WIN-EOM4PK0578N
|   NetBIOS_Domain_Name: WIN-EOM4PK0578N
|   NetBIOS_Computer_Name: WIN-EOM4PK0578N
|   DNS_Domain_Name: WIN-EOM4PK0578N
|   DNS_Computer_Name: WIN-EOM4PK0578N
|   Product_Version: 10.0.17763
|_  System_Time: 2023-07-04T11:44:33+00:00
| ssl-cert: Subject: commonName=WIN-EOM4PK0578N
| Not valid before: 2023-07-03T11:38:09
|_Not valid after:  2024-01-02T11:38:09
|_ssl-date: 2023-07-04T11:44:37+00:00; -1s from scanner time.
8021/tcp open  freeswitch-event FreeSWITCH mod_event_socket

## Samba server
``` mount_smbfs -N //guest@10.10.22.244/websvr mnt/ ```

