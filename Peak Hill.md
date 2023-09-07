# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx peakhill.thm
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
sudo nmap -T4 -p- -sV peakhill.thm
```

we got these open ports
```bash
PORT     STATE  SERVICE  VERSION
20/tcp   closed ftp-data
21/tcp   open   ftp      vsftpd 3.0.3
22/tcp   open   ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
7321/tcp open   swx?
```

# FTP (port 21)
the .creds file is a python3 pickle file


┌──(kali㉿kali)-[~/thm/Peak Hill]
└─$ python3 unpickle.py
['ssh_pass0', 'ssh_pass1', 'ssh_pass10', 'ssh_pass11', 'ssh_pass12', 'ssh_pass13', 'ssh_pass14', 'ssh_pass15', 'ssh_pass16', 'ssh_pass17', 'ssh_pass18', 'ssh_pass19', 'ssh_pass2', 'ssh_pass20', 'ssh_pass21', 'ssh_pass22', 'ssh_pass23', 'ssh_pass24', 'ssh_pass25', 'ssh_pass26', 'ssh_pass27', 'ssh_pass3', 'ssh_pass4', 'ssh_pass5', 'ssh_pass6', 'ssh_pass7', 'ssh_pass8', 'ssh_pass9', 'ssh_user0', 'ssh_user1', 'ssh_user2', 'ssh_user3', 'ssh_user4', 'ssh_user5', 'ssh_user6']
gherkin
p1ckl3s_@11_@r0und_th3_w0rld

we can connect to with ssh



# uncompyle6 version 3.5.0
# Python bytecode 3.8 (3413)
# Decompiled from: Python 2.7.5 (default, Jun 20 2023, 11:36:40) 
# [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)]
# Embedded file name: ./cmd_service.py
# Size of source mod 2**32: 2140 bytes
from Crypto.Util.number import bytes_to_long, long_to_bytes
import sys, textwrap, socketserver, string, readline, threading
from time import *
import getpass, os, subprocess
username = long_to_bytes(1684630636)
password = long_to_bytes(2457564920124666544827225107428488864802762356L)

class Service(socketserver.BaseRequestHandler):

    def ask_creds(self):
        username_input = self.receive('Username: ').strip()
        password_input = self.receive('Password: ').strip()
        print(username_input, password_input)
        if username_input == username:
            if password_input == password:
                return True
            return False

    def handle(self):
        loggedin = self.ask_creds()
        if not loggedin:
            self.send('Wrong credentials!')
            return
        self.send('Successfully logged in!')
        while True:
            command = self.receive('Cmd: ')
            p = subprocess.Popen(command,
              shell=True, stdout=(subprocess.PIPE), stderr=(subprocess.PIPE))
            self.send(p.stdout.read())

    def send(self, string, newline=True):
        if newline:
            string = string + '\n'
        self.request.sendall(string)

    def receive(self, prompt='> '):
        self.send(prompt, newline=False)
        return self.request.recv(4096).strip()


class ThreadedService(socketserver.ThreadingMixIn, socketserver.TCPServer, socketserver.DatagramRequestHandler):
    pass


def main():
    print('Starting server...')
    port = 7321
    host = '0.0.0.0'
    service = Service
    server = ThreadedService((host, port), service)
    server.allow_reuse_address = True
    server_thread = threading.Thread(target=(server.serve_forever))
    server_thread.daemon = True
    server_thread.start()
    print('Server started on ' + str(server.server_address) + '!')
    while True:
        sleep(10)


if __name__ == '__main__':
    main()

by using : https://www.toolnb.com/tools-lang-fra/pyc.html

    dill
    n3v3r_@_d1ll_m0m3nt



/home/dill/.config/lxc/client.key
