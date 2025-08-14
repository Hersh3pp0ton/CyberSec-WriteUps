### [https://tryhackme.com/room/lazyadmin]

# IP: 10.10.6.35

- # ENUM
	- NMAP 
		-   ``` PORT   STATE SERVICE VERSION
			22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
			| ssh-hostkey: 
			|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
			|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
			|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
			80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
			|_http-server-header: Apache/2.4.18 (Ubuntu)
			|_http-title: Apache2 Ubuntu Default Page: It works
			Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel ```
	- GOBUSTER
		- `/content
			- `/_themes
			- `/as
			- `/attachment1
			- `/images
			- `/inc
			- `/js
		- `/index.html


- # EXPLOITATION
	- Check SweetRice on searchsploit (Backup Disclosure)
		- username: manager
		- password (hash): 42f749ade7f9e195bf475f37a44cafcb
		- password: Password123


