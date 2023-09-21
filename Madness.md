# #1 Introduction

<img width="647" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/ece06d4c-dce7-4e4d-ab71-0bec9356a190">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx madness.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- madness.thm
```

we got these open ports
```bash

```

# #3 user.txt

## HTTP (port 80)
we found an image in source code but it's a rabbit hole
<img width="686" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/19a61e44-4803-4d84-bb0f-cb97837c59e6">

<img width="1120" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/d0e11677-7aee-4e69-ac32-4cd22051a0d3">

<img width="515" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/d333a065-d54b-4d48-9984-1cbc6e482815">

<img width="653" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/b8b72e32-7354-4b9e-abba-b28f9ca97050">

<img width="1329" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/785fe3de-050f-42d7-91c1-006d7cdbb354">

joker:*axA&GF8dP

http://madness.thm/th1s_1s_h1dd3n/?secret=73
<img width="659" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/f2657c5b-1074-419a-b2a2-2a2685b781d7">

<img width="831" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/614d966c-abe0-4a24-b59f-84317971695c">

y2RPJ4QaPF!B

we can abuse the screen binary with suid that is in 4.5.0 version
https://github.com/XiphosResearch/exploits/blob/master/screen2root/screenroot.sh

