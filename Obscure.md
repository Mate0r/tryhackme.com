# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx obscure.thm
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
sudo nmap -T4 -p- -sV obscure.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Werkzeug httpd 0.9.6 (Python 2.7.9)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```


# FTP (port 21)
we can connect as anonymous


antisoft.thm


```
void pass(char *param_1)
{
  int iVar1;
  long in_FS_OFFSET;
  undefined8 local_28;
  undefined8 local_20;
  undefined2 local_18;
  undefined local_16;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_28 = 0x6150657275636553;
  local_20 = 0x323164726f777373;
  local_18 = 0x2133;
  local_16 = 0;
  iVar1 = strcmp(param_1,"971234596");
  if (iVar1 == 0) {
    printf("remember this next time \'%s\'\n",&local_28);
  }
  else {
    puts("Incorrect employee id");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return;
}




undefined8 main(void)

{
  long in_FS_OFFSET;
  undefined local_28 [24];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  puts("Password Recovery");
  puts("Please enter your employee id that is in your email");
  __isoc99_scanf(&DAT_0040088c,local_28);
  pass(local_28);
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```





971234596
SecurePassword123!


```
COPY public.res_users (id, active, login, password, company_id, partner_id, create_date, share, write_uid, create_uid, action_id, write_date, signature, password_crypt) FROM stdin;
3	f	default		1	4	2022-07-23 10:51:25.449364	f	1	1	\N	2022-07-23 10:51:25.449364	\N	\N
4	f	public	\N	1	5	2022-07-23 10:51:25.449364	t	1	1	\N	2022-07-23 10:51:25.449364	\N	\N
1	t	admin@antisoft.thm		1	3	\N	f	1	\N	\N	2022-07-23 10:52:10.087949	<span data-o-mail-quote="1">-- <br data-o-mail-quote="1">\nAdministrator</span>	$pbkdf2-sha512$12000$lBJiDGHMOcc4Zwwh5Dzn/A$x.EZ/PrEodzEJ5r4JfQo2KsMZLkLT97xWZ3LsMdgwMuK1Ue.YCzfElODfWEGUOc7yYBB4fMt87ph8Sy5tN4nag
```

we can use admin@antisoft.thm with password SecurePassword123! to connect to webapp
maybe try to download database anonymisation from odoo.com and install it manually

we have a wepapp odoo in version 10.0-20190816
we can exploit the webapp with plugin database anonymation


# set the postgres database host, port, user and password according to the environment
# and pass them as arguments to the odoo process if not present in the config file
: ${HOST:=${DB_PORT_5432_TCP_ADDR:='db'}}
: ${PORT:=${DB_PORT_5432_TCP_PORT:=5432}}
: ${USER:=${DB_ENV_POSTGRES_USER:=${POSTGRES_USER:='odoo'}}}
: ${PASSWORD:=${DB_ENV_POSTGRES_PASSWORD:=${POSTGRES_PASSWORD:='odoo'}}}

there is a suid file in /ret



rax            0x7fffffffe990      140737488349584
rbx            0x0                 0
rcx            0x7ffff7fbca00      140737353861632
rdx            0x7ffff7fbe8d0      140737353869520
rsi            0x602671            6301297
rdi            0x7fffffffe991      140737488349585
rbp            0x7fffffffea00      0x7fffffffea00
rsp            0x7fffffffea18      0x7fffffffea18
r8             0x6026f1            6301425
r9             0x0                 0
r10            0x602010            6299664
r11            0x246               582
r12            0x400550            4195664
r13            0x7fffffffeb00      140737488349952
r14            0x0                 0
r15            0x0                 0
rip            0x4006bd            0x4006bd <vuln+72>
eflags         0x246               [ PF ZF IF ]
cs             0x33                51
ss             0x2b                43
ds             0x0                 0
es             0x0                 0
fs             0x0                 0




rax            0x7fffffffe990      140737488349584
rbx            0x0                 0
rcx            0x7ffff7fbca00      140737353861632
rdx            0x7ffff7fbe8d0      140737353869520
rsi            0x602671            6301297
rdi            0x7fffffffe991      140737488349585
rbp            0x7f0061616161      0x7f0061616161
rsp            0x7fffffffea18      0x7fffffffea18
r8             0x6026f5            6301429
r9             0x0                 0
r10            0x602010            6299664
r11            0x246               582
r12            0x400550            4195664
r13            0x7fffffffeb00      140737488349952
r14            0x0                 0
r15            0x0                 0
rip            0x4006bd            0x4006bd <vuln+72>
eflags         0x246               [ PF ZF IF ]
cs             0x33                51
ss             0x2b                43
ds             0x0                 0
es             0x0                 0
fs             0x0                 0


https://beta.hackndo.com/retour-a-la-libc/

